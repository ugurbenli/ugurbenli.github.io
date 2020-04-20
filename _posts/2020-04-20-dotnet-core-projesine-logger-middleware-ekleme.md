---
layout: post
lang: tr
title: .Net Core projesine Logger Middleware ekleme
blog: yes
tags: [dotnet core, .net core, logger, serilog, custom middleware]
---

Merhabalar,

Önceki yazımızda oluşturduğumuz .Net Core projesine eklemelere ilk olarak log middleware ekleyerek başlamayı düşündüm. Yazıya aşağıdaki [linkten](https://www.ugurbenli.com/powershell-de-dotnet-core-projesi-olusturma/ "Powershell'de .Net Core projesi oluşturma"){:target="_blank"} ulaşabilirsiniz.

Konumuza geri dönersek hali hazırda bir Log yapısı .Net Core ile birlikte geliyor ancak ben daha esnek olması sebebiyle ***Serilog*** kullanacağım. Burada amacımız çalışma zamanında olaşabilecek exception'ları tek bir yerden yakalayıp loglamak olacak.

Terimleri açıklamak gerekirse;

{: .box-note}
**Middleware**; Request-Response arasındaki akış sürecini delegate'ler aracılığıyla ana süreçten (**Pipeline**) devralıp istenilen işlemleri uygulayan yapılardır. İşlemleri bitince kontrolü geri **Pipeline**'a verir.

{: .box-note}
**Pipeline**; Request'ten Response'a kadar yürütülen sürecin tamamını kapsayan ve içerisinde middleware'lere ev sahipliği yapan ana yapıdır.

Aşağıda bu yapının figürünü görebilirsiniz.

<a href="/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/netcore-pipeline.png" rel="Resmi büyütmek için tıklayın">
![.Net Core Pipeline](/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/netcore-pipeline.png ".Net Core Pipeline")
</a>

Şimdi dilerseniz işlemlerimize geçelim. Şunları yapacağız;

- Web API projemize ***Serilog*** paketini ekleyeceğiz.
- Serilog için appsettings düzenlemesi yapacağız. Ortamınıza göre değişiklik gösterecektir bu dosya, burada biz Development ortamına ayar yapacağız.
- Eklediğimiz paketi kullanarak bir middleware oluşturacağız.
- Projemize ***Serilog*** ayarlarını ve **Middleware**'imizi ekleme
- Ve sonunda test 🙂

### Serilog paketini projeye dahil etme

[Nuget](https://www.nuget.org "Nuget"){:target="_blank"} anasayfasından Serilog paketlerine göz atabiliriz. ***Serilog***'un bu yazıyı yazarkenki son kararlı sürümünü indiriyorum.

`dotnet add package Serilog.AspNetCore --version 3.2.0`

Ayrıca loglama türünüze göre değişkenlik gösterecek olan yardımcı paketi de indirmeliyiz. Örneğin biz burada tek bir file üzerinden günlük loglama yapmak istediğimizden **RollingFile** paketini indiriyoruz. Sizin yapacağınız log türüne göre indireceğiniz paket farklı olacaktır.

`dotnet add package Serilog.Sinks.RollingFile --version 3.3.0`

### Serilog Paketi için AppSettings Düzenlemesi

{: .box-note} 
Development ortamı kullanacağım için **appsettings.Development.json** dosyasında işlem yapıyorum. Ortamınıza göre bu dosyanın, Test, Production gibi isimler alabileceğini unutmayın.

Bu örnekte ben logları proje altında Logs klasörü içine eklemek istedim.

```Json
{
  "Serilog": {
    "MinimumLevel": "Information",
    "WriteTo": [
      {
        "Name": "RollingFile",
        "Args": {
          "pathFormat": "Logs/log-{Date}.txt",
          "outputTemplate": "{Timestamp:yyyy-MM-dd HH:mm:ss.fff zzz} [{Level}] [{SourceContext}] [{EventId}] {Message}{NewLine}{Exception}"
        }
      }
    ]
  }
}

```

### Middleware Oluşturma

Web API projemizde Middleware klasörü ve içerisinde bir middleware classı oluşturuyoruz. Ben burada ismine **GlobalExceptionMiddleware** diyeceğim.

İlk olarak requestleri handle etmek için **RequestDelegate** classından yararlanacağımızı söylemek istiyorum, bu delegate classı .Net Core içerisinde ***Microsoft.AspNetCore.Http*** ile birlikte geliyor ve her request öncesinde o request'i handle edebilmemizi sağlıyor. Biz de bundan bir instance almak için **Dependency Injection (DI)** 'a başvuracağız.

```C#

private readonly RequestDelegate _request;

public GlobalExceptionMiddleware(RequestDelegate request)
{
    _request = request;
}

```

Ardından Serilog'dan bir nesne alacağız. Bunu da Serilog'un **ForContext** generic metodunu kullanarak yapabiliriz. Bu metod bize **ILogger** interface'ini implement etmiş bir Logger örneği verecek.

```C#

private static readonly ILogger _logger = Log.ForContext<GlobalExceptionMiddleware>();

```

Son adımda classımız şu şekilde olmalı;

```C#

private readonly RequestDelegate _request;
private static readonly ILogger _logger = Log.ForContext<GlobalExceptionMiddleware>();

public GlobalExceptionMiddleware(RequestDelegate request)
{
    _request = request;
}

```

Şimdi de her middleware classının zorunlu olarak sahip olması gereken **InvokeAsync** isimli metodunu eklemeliyiz. Bu metod **Pipeline** tarafından bilinen ve kodun yönetiminin bize verildiği metoddur. **Task** tipinde dönüş tipi ve **HttpContext** türünde de parametresi olmalıdır. Burada global bir **try-catch** mekanizması ile hatamızı yakalayabiliriz.

```C#

public async Task InvokeAsync(HttpContext httpContext)
{
    try
    {
        await _request(httpContext);
    }
    catch (Exception ex)
    {
        // Kalabalığı engellemek için hata logic'ini bir metoda taşıyorum.
        await HandleExceptionAsync(httpContext, ex);
    }
}

private static Task HandleExceptionAsync(HttpContext httpContext, Exception ex)
{
    _logger.Error($"{DateTime.Now.ToString("HH:mm:ss")} : {ex}");
    
    return Task.CompletedTask;
}

```


#### Middleware'imiz son halini aldı. Burada adımları örnek senaryo ile tekrar etmek gerekirse;

- Kullanıcı tarafından bir request gelir ve request middleware'imizdeki **try-catch** tarafından kontrollü olarak değerlendirmeye alınır.
- Bu değerlendirme sırasında kodumuz hataya düşer ve catch bloğumuz bu hatayı loglaması için **HandleExceptionAsync** metodunu çağırır ve hata loglanır.

### Logger ve Middleware'i projemize bildirme

Kodlarımızı yazdık, iyi ama bundan uygulamamızın haberi yok onu haberdar etmek de bizim görevimiz tabii 🙂 Bunun için
**Program.cs** ve **Startup.cs** classlarımızda ayarlar yapacağız.

#### Program.cs

**CreateHostBuilder** metodu içerisinde aşağıdaki gibi bir düzenleme yapıyoruz. Bu düzenleme Serilog'u ayarlarını appsettings üzerinden alacak şekilde projemize dahil etmemizi sağlıyor.

```C#

public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
            
            //Serilog'u ayarları appsettings'ten alacak şekilde ayarlıyoruz.
            webBuilder.UseSerilog((ctx, config) => { config.ReadFrom.Configuration(ctx.Configuration); });
        });

``` 

#### Startup.cs

```C#

public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    // Middleware'imizi projemize tanıtıyoruz.
    app.UseMiddleware<GlobalExceptionMiddleware>();  
}

``` 

### Test 🙂

Şimdi yalandan bir exception çıkarıp sonuçlarına hep birlikte bakalım. Bunun için örnek controller classımız olan WeatherForecast'e yanlış çalışma ihtimali olan bir kod eklemesi yapacağım. Şu şekilde;

```C#

[HttpGet]
public IEnumerable<WeatherForecast> Get([FromRoute]int a)
{
    // Acaba burada a'ya sıfır(0) gelirse nolur dersiniz? DivideByZeroException hemen enseler tabii :)
    var b = 5 / a;

    ...
}

```

Vee işte hata anı

<a href="/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/exception.png" rel="Resmi büyütmek için tıklayın">
![Hata](/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/exception.png "Hata")
</a>

Tabii ki logs folderımız ve dosyamız oluşmuş mu ona bakalım 🙂

<a href="/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/logs-folder.png" rel="Resmi büyütmek için tıklayın">
![Logs folder](/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/logs-folder.png "Logs folder")
</a>

Folderda yer alan dosyamızın içeriği şu şekilde;

<a href="/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/logs-file.png" rel="Resmi büyütmek için tıklayın">
![Logs file](/img/2020-04-20-dotnet-core-projesine-logger-middleware-ekleme/logs-file.png "Logs file")
</a>

Eveeet, sonunda benim için güzel ve bir o kadar da zevkli bir yazının sonuna geldik, umarım sizin için de öyle olmuştur. Proje kaynak kodlarını [buradan](https://github.com/ugurbenli/dotnetcoreapivscodeexample ".Net Core Example Project"){:target="_blank"} inceleyebilir ve kaynak kodlarına erişebilirsiniz. 

İleriki yazılarda bu projemizi büyütüp genişleteceğiz, takipte kalın 😊

Hoşça kalın 🖐