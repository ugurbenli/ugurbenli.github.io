---
layout: post
lang: tr
title: .Net Core projesine Swashbuckle (Swagger) ekleme
blog: yes
tags: [dotnet core, .net core, swashbuckle, swagger, documentation, dokümantasyon]
---

Merhabalar,

Önceden oluşturduğumuz .Net Core projesine yeni eklemelere devam ediyoruz. Eğer proje oluşturma adımını kaçırdıysanız yazıya şuradaki [linkten](https://www.ugurbenli.com/powershell-de-dotnet-core-projesi-olusturma/ "Powershell'de .Net Core projesi oluşturma"){:target="_blank"} ulaşabilir, seriye siz de katılabilirsiniz.

![Swagger](/img/2020-06-01-dotnet-core-projesine-swashbuckle-swagger-ekleme/swagger-logo.png "Swagger")

Öncelikle Swashbuckle nedir? Swagger nedir? gibi soruların cevaplarını vereyim.

{: .box-note}
**Swashbuckle**; Swagger oluşturmak için kullanılan temel kütüphanedir.

{: .box-note}
**Swagger**; Yazılımcılar için RESTful API'lerde kullanılmak üzere geliştirilmiş, projedeki servislerin dokümante edilmiş halidir. Servislere nasıl gidilip, nasıl cevap alınacağı gibi detaylara sahiptir.

Swagger temelde 3 Nuget paketinden oluşuyor. Swashbuckle kütüphanesi bu 3 pakette de kullanılmaktadır. Paketler şunlardır;

- **Swashbuckle.AspNetCore.Swagger** : Swagger'ın ana JSON modeli olan SwaggerDocument isimli modeli içerir. Bu model dokümanın yorumlanmamış ham halidir. 

- **Swashbuckle.AspNetCore.SwaggerGen** : SwaggerDocument oluşturulmasını sağlar. Bu paket sayesinde projemizde yer alan route, controller ve modeller SwaggerDocument olarak dokümante edilir.

- **Swashbuckle.AspNetCore.SwaggerUI** : SwaggerDocument'i yorumlayan pakettir, request-responseların Swagger önyüzü aracılığıyla gerçekleştirilmesini etkin hale getirir.

Swashbuckle, Swagger hakkında bilgi vermeye çalıştım. Akılda kalan soruları bir örnek yaparak gidermeye çalışayım. Bunun için, adımlarım;

- Nuget üzerinden Swagger paketlerini indirmek
- Startup.cs üzerinde gerekli ayarlamalar
- Test işlemi 🙂

şeklinde olacak.

### Swagger paketlerini indirmek

Eklenecek paketlerin isimleri;

- **Swashbuckle.AspNetCore.SwaggerGen** (**Swashbuckle.AspNetCore.Swagger** paketine bağımlı olduğundan onu da indiriyor.)
- **Swashbuckle.AspNetCore.SwaggerUI**

{: .box-note}
**Visual Studio için**; Nuget paket yöneticisi ile paketler projeye eklenebilir.

<a href="/img/2020-06-01-dotnet-core-projesine-swashbuckle-swagger-ekleme/swagger-packages.png" rel="Resmi büyütmek için tıklayın">
![Swagger Nuget Paketleri](/img/2020-06-01-dotnet-core-projesine-swashbuckle-swagger-ekleme/swagger-packages.png "Swagger Nuget Paketleri")
</a>

{: .box-note}
**Visual Studio Code için**; Powershell üzerinden paket ekleme komutu olan `dotnet add package <PaketIsmi>` kullanılarak paketler projeye eklenebilir.

### Startup.cs ayarları

Swagger projeye kolayca dahil olabilir bir yapıya sahiptir, isteğe göre özelleştirilebilir şekildedir. Bu özelleştirmelere örnek olarak API'da bir token yapısı kullanılıyorsa bu yapıyı Swagger'a tanıtmak birkaç dakika alacaktır. Burada ham olarak default haliyle projeye ekliyorum. Bu ekleme için ***Configure*** ve ***ConfigureServices*** metodlarına bazı eklemeler yapılmalıdır.

***Configure*** metodu için;

```C#

// Swagger doküman dosyasının dahil edilmesi (SwaggerDocument)
app.UseSwagger();

// SwaggerGen tarafından oluşturulan dokümanın yorumlanması işleminin dahil edilmesi
app.UseSwaggerUI(c =>
{
    c.SwaggerEndpoint("/swagger/v1/swagger.json", "DotnetCoreApiVsCodeExample.API V1");
});

```

***ConfigureServices*** metodu için;

```C#

// Swagger doküman dosyasını oluşturan service
services.AddSwaggerGen(c =>
{
    c.SwaggerDoc("v1", new OpenApiInfo { Title = "DotnetCoreApiVsCodeExample.API", Version = "v1" });
});

```

Son durumda Startup.cs aşağıdaki şekilde olmalıdır. Gereken ayarlar sadece bunlar, şimdi test işlemine geçebiliriz.

<script src="https://gist.github.com/ugurbenli/532f42e3994d33cf61e8854053e8b085.js"></script>

### Test işlemi 🙂

Swagger arayüzüne **/swagger/index.html** route'uyla erişilebilir. Şöyle bir ekranla karşılaşıldıysa işlem başarılıdır.

<a href="/img/2020-06-01-dotnet-core-projesine-swashbuckle-swagger-ekleme/swagger-document.png" rel="Resmi büyütmek için tıklayın">
![Swagger Ekranı](/img/2020-06-01-dotnet-core-projesine-swashbuckle-swagger-ekleme/swagger-document.png "Swagger Ekranı")
</a>

### Sonuç

Bu yazımda Swagger'ı incelemeye ve sizi bilgilendirmeye çalıştım. Swagger sizi doküman yazmak gibi çoğu kişinin zor iş olarak tanımladığı yükten kurtaran bir araç, kullanması da oldukça basit. 

Bu örnek projeyi sürekli olarak geliştirmeye çalışıyorum. Proje kaynak kodlarını aşağıdaki linkten inceleyebilir ve tüm kaynak kodlarına erişebilirsiniz.

[Proje Github Sayfası](https://github.com/ugurbenli/dotnetcoreapivscodeexample "Proje Github Sayfası"){:target="_blank"}

Mutlu kodlamalar, takipte kalın 😊

Hoşça kalın 🖐

#### Kaynaklar

- [Get started with Swashbuckle and ASP.NET Core](https://docs.microsoft.com/en-us/aspnet/core/tutorials/getting-started-with-swashbuckle?view=aspnetcore-3.1&tabs=visual-studio "Get started with Swashbuckle and ASP.NET Core"){:target="_blank"}

<br/>