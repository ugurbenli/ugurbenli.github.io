---
layout: post
lang: tr
title: Powershell'de .Net Core projesi oluşturma
blog: yes
tags: [powershell, vs code, dotnet core, .net core]
---

Merhabalar,

Sizlere ilk blog yazımda .Net Core'dan bahsetmek istedim.
 
> .Net Core kısaca; Microsoft tarafından geliştirilen açık kaynaklı, ortam bağımsızlığını destekleyen bir ***SDK***'dır.
 
Aramızda .Net Core ile ilgili bilgi sahibi olmayan varsa hemen şu [linkten](https://docs.microsoft.com/tr-tr/dotnet/core/ ".NET Core Documentation"){:target="_blank"} detaylı bilgi sahibi olabilir.

![.Net Core](/img/2020-04-10-powershell-de-dotnet-core-projesi-olusturma/netcore.png ".Net Core")

Konumuza geri dönersek, .Net'i hep ***Visual Studio*** ortamında kullanmaya alışığız diye düşünüyorum en azından ben buna alışığım. Devir gelişim ve değişim devri olduğundan bu alışkanlığın üstüne gitmek istedim, benimle aynı düşüncede olanlar vardır diye düşünüyorum 🙂

Bu yazımızdaki adımlarımız basitçe şunlar olacak;

* Projemiz için öncelikle bir Solution(SLN) dosyası oluşturacağız. Bu bilmeyenler için kısaca projelerimizin ana sarıcısıdır, yani bu dosyaya proje dosyalarımızı, paket dosyalarımızı vb. bağlarız.
* Web API projesi oluşturacağız.
* Oluşturduğumuz solutiona oluşturduğumuz Web API projesini ekleyeceğiz.

> Öncelikle bilgisayarınızda .Net Core yüklü olduğundan emin olun. Bilgisayarınızda .Net Core'un olmadığını öğrenmek için `dotnet --version` komutunu kullanabilirsiniz. 

Ben console olarak ***PowerShell*** kullanacağım. Kolay olması için ***VS Code*** içerisinde yer alan ***PowerShell***'i kullanabiliriz. Komutlarla ilgili detaylı bilgiyi isterseniz ***PowerShell***'den ya da yukarıda belirttiğim web sitesi üzerinden alabilirsiniz. 

***PowerShell***'de detaylı bilgi almak için;

`dotnet <<komut>> --help`

#### Solution Dosyası Oluşturma

Solution dosyamızı aşağıdaki komutla oluşturuyoruz. İsmini ise `--name` parametresiyle verebiliyoruz. Burada ben **DotnetCoreApiVsCodeExample** ismini verdim.

`dotnet new sln --name DotnetCoreApiVsCodeExample`

#### Web API Projesi Oluşturma

Web Api projesi olarak da Microsoft'un bize örnek olarak verdiği API'yi oluşturması için aşağıdaki komutu kullanıyoruz. İsmini ise yine `--name` parametresiyle verebiliyoruz. Burada **DotnetCoreApiVsCodeExample.API** ismiyle oluşturdum.

`dotnet new webapi --name DotnetCoreApiVsCodeExample.API`

#### Solution içine projemizi dahil etme

İşte son adımımıza geldik. Şu aşamada solution ve projemizin birbirinden haberi yok. Bu arkadaşları haberdar etmek lazım 🙂 Bunun için şu komutu kullanıyoruz ve artık birbirlerini öğreniyorlar.

`dotnet sln .\DotnetCoreApiVsCodeExample.sln add .\DotnetCoreApiVsCodeExample.API\`

Evet, basitçe ***VS Code*** içerisindeki ***Powershell***'de uygulama oluşturarak bu uygulamayı bir solution dosyasına bağladık. İleriki yazılarda bu solution'a başka şeyleri de nasıl ekleyeceğimizi göreceğiz.

Hoşça kalın 🖐
