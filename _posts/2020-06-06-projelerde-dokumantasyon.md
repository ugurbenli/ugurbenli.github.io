---
layout: post
lang: tr
title: Dokümantasyon ve projede önemi
blog: yes
tags: [documentation, dokümantasyon, project management, proje yönetimi]
---

Merhabalar,

Bugünkü yazımda yer aldığım projelerde gördüğüm ve projenin adaptasyon sürecini, anlaşılmasını zorlaştıran bir sorundan bahsetmek istiyorum. “Nedir bu sorun?” derseniz tabii ki de konu başlığında da belirttiğim gibi **dokümantasyon**.

![Dokümantasyon ve projede önemi](/img/2020-06-06-projelerde-dokumantasyon/telescope.jpg "Dokümantasyon ve projede önemi")

### Dokümantasyon nedir?

Dokümantasyon, kelime olarak belgelendirme anlamına gelir. Bir konuyu veya projeyi belgelere dayandırarak gelecek kuşaklara, kişilere aktarımını sağlamak olarak ifade edilebilir.

### Dokümantasyon olmayan proje nasıl olur?

Örneğin bir projeye katılacaksınız, bu bir yazılım projesi ve geliştiren arkadaşlar projeyle ilgili en ufak bir dokümantasyon girişiminde bulunmamış. Yani projeyi yazıp geçmişler sektör tabiriyle. Şimdi akıldaki sorular şöyle olacaktır:

- Siz bu projeye nasıl adapte olacaksınız?
- Adapte olma yolunda kimden destek alabileceksiniz?
- Destek alsanız dahi her istediğinizde sorunuzu sorabilecek misiniz?

Bunlar can sıkan sorular maalesef… Daha projeye girip destek olmaya başlamadan aşmanız gereken bir tümsek gibi karşınıza çıkıyor. Bu soruları çözmekte muhatap olarak genelde kimseyi bulamazsınız çünkü ya çekirdek yapıyı yazan arkadaş işten ayrılmıştır, ki bu güçlü bir ihtimal, ya da belki de yazdığı o yapıyı çoktan unutmuştur🙂. 

Bu durumda ne yapacağız? Oturup tek tek kodları incelemek, hiç bilgi sahibi olmadan teleskopla uzayı seyretmeye benzer. Yani bir şeyler görürsünüz, çok da hoşunuza gider ama ne olduğunu bilemez, detaylarına inemezsiniz. Üstelik detaylara indikçe daha keşfedilmeyi bekleyen çok şey vardır, öğrenilmesi gereken çok şey olacaktır ancak siz tüm bunlardan mahrum kalırsınız. 

#### Buna sebebiyet vermemek için ne yapmalı?

Böylesine bir para, zaman israfına sebebiyet vermemek için dokümantasyonun önemi apaçık ortadadır. Yapılması gereken zamanında yani daha bilginiz tazeyken, projeyi dokümante ederek hem proje bilginizi oturtmak, hem de projeye katılacak olan arkadaşlara bir miras niteliğinde doküman bırakmaya çalışmaktır.

### Peki nasıl yapmalı?

Doküman olmayan projenin bu dezavantajlarından kurtulmak için doküman yazmaya karar verdiniz. İyi ama nasıl yazmalısınız? Bu konuda şu kuralları takip edebilirsiniz:

- Açıklayıcı olunmalı, sadece sizin anladığınız şekilde değil herkesin anlayacağı bir dille yazılmalı
- Gereksiz uzatmalara yer verilmemeli
- Açık uçlu sorulara sebebiyet verilmemeli
- Bir indeks sayfası hazırlanmalı ve konu başlıkları iyi belirlenmeli

Uzun uzadıya doküman yazılamıyorsa, en azından kod içerisine summary (özet) açıklamalar yazılmalı. Yorum satırlarına aşırı sebebiyet veren kodlar refactor edilmeli (yeniden düzenlenmeli) ve kısa açıklamalarla desteklenmeli. Bu şekilde de proje yarı dokümante edilebilir. Hem doküman yazılıp hem de bahsettiğim şekilde kodlara notlar alırsanız gerçekten dokümante edilmiş bir projeniz olacaktır.

Örneğin aşağıdaki gibi yorumlar açıklayıcı olacaktır.

<script src="https://gist.github.com/ugurbenli/22747621c870a4b2ddd1643f7eb5036e.js"></script>

API projeleri için kod içindeki notları son kullanıcı için görselleştiren bir teknoloji mevcut. Bundan bir önceki yazımda bahsettim, oradan inceleyerek projelerinize betimleyici bir anlatım katabilirsiniz.

[.Net Core projesine Swashbuckle (Swagger) ekleme](https://www.ugurbenli.com/dotnet-core-projesine-swashbuckle-swagger-ekleme ".Net Core projesine Swashbuckle (Swagger) ekleme")

### Dokümantasyon sürecini olumsuz etkileyen faktörler

İster bir şirket ister bir geliştirici olsun eğer kolaya kaçmak istenmiyorsa doküman yazmak ağır bir iş olmayacaktır. Aşağıda yer alan faktörler bu süreci ya başlamadan ya da süreç içerisindeyken olumsuz etkileyebilir. Bunlar:

**Şirket faktörleri**

- Sürecin bir para ve zaman kaybı olarak görülmesi
- Baskıyla sürecin hızlandırılmak istenmesi

**Geliştirici faktörleri**

- Gereksiz bir iş yükü yani angarya olarak görülmesi
- Üstünde fazla baskı ya da rahatlık hissederek süreçten soğuması

Gördüğünüz gibi bu konuda belli başlı faktörler var. Bunlar sürecin sıkıcı bir hal almasına sebep vermektedir. 

### Sonuç

“Dokümantasyon nedir?” sorusuyla başladığım yazımda bunun önemini açıklamaya ve “Eğer yapılmazsa neler olur?”, “Nasıl yapılmalı?” gibi soruların cevaplarına değinmeye çalıştım. Bu süreçten kaçarak belki kısa vadede kazanan taraf olabilirsiniz ancak ne şirketler ne de çalışanlar uzun vadede kazanan taraf olmayacaktır. Şimdi kazandığınız para ya da zaman ileride daha büyük bir iş yükü olarak karşınıza çıkacaktır. Bu sürece zaman ayırmalı, projelerimizde yer vermeliyiz ki temiz ve anlaşılır işler ortaya çıksın.

Mutlu kodlamalar, takipte kalın 😊

Hoşça kalın 🖐

#### Kaynaklar

- [XML documentation comments (C# programming guide)](https://docs.microsoft.com/tr-tr/dotnet/csharp/programming-guide/xmldoc/ "XML documentation comments (C# programming guide)"){:target="_blank"}

