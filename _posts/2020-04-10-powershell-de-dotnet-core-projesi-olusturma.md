---
layout: post
lang: tr
title: Powershell'de .Net Core projesi oluşturma
blog: yes
tags: [powershell, vs code, dotnet core, .net core]
---

Merhabalar,

Sizlere ilk blog yazımda .Net Core'dan bahsetmek istedim. .Net Core kısaca; Microsoft tarafından geliştirilen açık kaynaklı, ortam bağımsızlığını destekleyen bir ***SDK***'dır. Aranızda .Net Core ile ilgili bilgi sahibi olmayan varsa hemen şu [linkten](https://docs.microsoft.com/tr-tr/dotnet/core/ ".NET Core Documentation"){:target="_blank"} detaylı bilgi sahibi olabilir.

![.Net Core](/img/2020-04-10-powershell-de-dotnet-core-projesi-olusturma/netcore.png ".Net Core")

Konumuza geri dönersek, .Net'i hep ***Visual Studio*** ortamında kullanmaya alışığız diye düşünüyorum en azından ben buna alışığım. Devir gelişim ve değişim devri olduğundan bu alışkanlığın üstüne gitmek istedim, benimle aynı düşüncede olanlar vardır diye düşünüyorum 🙂

Bu yazımızdaki adımlarımız basitçe şunlar olacak;

- Projemiz için öncelikle bir Solution(SLN) dosyası oluşturacağız. Bu bilmeyenler için kısaca projelerimizin ana sarıcısıdır, yani bu dosyaya proje dosyalarımızı, paket dosyalarımızı vb. bağlarız.
- .Net Core Web API projesi oluşturacağız.
- Oluşturduğumuz Solution(SLN)'a oluşturduğumuz Web API projesini dahil edeceğiz.

Öncelikle bilgisayarınızda .Net Core yüklü olduğundan emin olun. Ben console olarak ***PowerShell*** kullanacağım. Kolaylık olarak ***VS Code*** içerisinde yer alan ***PowerShell***'i kullanabiliriz. Komutlarla ilgili detaylı bilgiyi isterseniz ***PowerShell***'den isterseniz de web sitesi üzerinden alabilirsiniz. 

Powershell'de detaylı bilgi almak için;

`dotnet <<komut>> --help`

#### Solution(SLN) Dosyası Oluşturma

Solution dosyamızı aşağıdaki komutla oluşturuyoruz. İsmini ise `--name` parametresiyle verebiliyoruz. Burada ben **DotnetCoreApiVsCodeExample** ismini verdim.

`dotnet new sln --name DotnetCoreApiVsCodeExample`

#### Web API Projesi Oluşturma

Web Api projesi olarak da Microsoft'un bize örnek olarak verdiği API'yi oluşturması için aşağıdaki komutu kullanıyoruz. İsmini ise yine `--name` parametresiyle verebiliyoruz. Burada **DotnetCoreApiVsCodeExample.API** ismiyle oluşturdum.

`dotnet new webapi --name DotnetCoreApiVsCodeExample.API`

#### SLN içine projemizi dahil etme

İşte son adımımıza geldik. Şu aşamada sln ve projemizin birbirinden haberi yok. Bu arkadaşları haberdar etmek lazım 🙂 Bunun için şu komutu kullanıyoruz ve artık birbirlerini öğreniyorlar.

`dotnet sln .\DotnetCoreApiVsCodeExample.sln add .\DotnetCoreApiVsCodeExample.API\`

Evet, basitçe ***VS Code*** içerisindeki ***Powershell***'de uygulama oluşturarak bu uygulamayı bir solution dosyasına bağladık. İleriki yazılarda bu solution'a başka şeyleri de nasıl ekleyeceğimizi göreceğiz.

Hoşça kalın 🖐