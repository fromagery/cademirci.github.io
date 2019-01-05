---
layout: post
excerpt: Jekyll dinamikleri ve tekniğine bir giriş, bazı temel kodlar ve açıklamalar.
tags: ['bilgisayar bilimleri', 'web tasarımı', 'jekyll']
image: https://cdn-images-1.medium.com/max/1200/0*N8RG95bKJnnF-wpL.png
title: Jekyll 2
---

### Jekyll 2

Serinin ilk yazısı: [Jekyll 1](caglayandemirci.github.io/jekyll-1)

Bir Jekyll sitesi kurmanın aşamalarını, gerek Jekyll'in [kendi sayfasında](https://jekyllrb.com/docs/), gerek YouTube videolarında ([1](https://www.youtube.com/watch?v=iWowJBRMtpc&t=160s), [2](https://www.youtube.com/watch?v=T1itpPvFWHI)) gerek Türkçe kaynak olarak [blog yazılarında](https://medium.com/@nafidurmus/jekyll-kullanarak-20-dakikada-blog-yap%C4%B1m%C4%B1-b2550043f455) bulabilirsiniz. Aynılarını getirip buraya yazmanın bir anlamı olmadığını düşündüm. Dolayısıyla bu kısmı geçiyorum.

### Sitenin Yapısı

Jekyll sitenizi kurduğunuzda, hiçbir oynama yapmadığınız siteniz içinde şunlarla yüklenir (sizin için Linux'ta tekrar bir Jekyll sitesi kurup ekran alıntısı aldım):

![](../caglayandemirci.github.io/images/post_images/default_jekyll.PNG)

Tabiki bütün bu klasörler ve içindeki dosyaların bir fonksiyonu, işe yaradıkları bir nokta var. Ama, meseleyi biraz daha basitleştirmek adına, ben bu yazı için temel dosyaları gösterecek şekilde bir tablo hazırladım. Sitenizde bunlar olduğu takdirde, web sayfanız çalışır. Hatta, daha fazlasını yapmasanız bile gayet profesyonel görünümlü ve iyi çalışan bir siteye sahip olabilirsiniz.

![](../caglayandemirci.github.io/images/post_images/jekyll_structure.png)

### Liquid

**Uyarı**: *Liquid, syntaxı gereği, iki adet açıp kapatılmış blok parantezi arasındaki değerleri ve bir { ve bir % ile açıp kapatılmış blokların içindeki işlemleri okur. Bu belgede onları { { } } ve { % % } şeklinde göstermek zorunda kaldım, aksi takdirde üzerinde bulunduğum Markdown sayfası bunları programlama çabası olarak algılayarak hata veriyor. Uyarım şudur ki, siz boşluk koymadan yazmalısınız... Evet, `code` bloklarının içine yazdığımda bile böyle saçma bir reaksiyon aldığım için GitHub Pages'e kırgınım.* 

Jekyll sayesinde, sitenizdeki HTML sayfalarının içinde Ruby ile yazılmış bir şablon dili (*template language*) olan Liquid ile programlama yapabilirsiniz. 

**`_includes`** ve **`{ % include % }`**:

`_includes`klasörünüzün içinde, temelde header ve footer, isterseniz de bin türlü başka sayfa parçanızı barındırabilirsiniz. Bunlar .html dosyaları olmalıdır. Liquid ile, herhangi bir sayfanın içinde bu sayfa parçalarını `{ % include ... % }`syntaxıyla kullanabilirsiniz. Örneğin:

```html
<html>
    { % include head.html % }
    <body>
        { % include header.html % }

        { % include footer.html % }
    </body>
</html>
```

**`_layouts`**:

Sırada, içinde içeriklerinizin de görüntüleneceği ve böylece sitenizin yüzü olacak sayfa şekilleriniz var.

```html
<html>
    <head>
        <title>{ { page.title } }</title>
    </head>
    { % include head.html % }
    <body>
        { % include header.html % }

        	{ { content } }

        { % include footer.html % }
    </body>
</html>
```

Bu, bir `post.hmtl` layout'u için uygun bir örnek. Bu sayfa, sitenizi yayına koyduğunuzda Markdown dosyanızın içine yazdığınız bütün içeriği alıp `{ { content } }` 'in yerine koyacak. Bunun ne demek olduğunu son Liquid örneğimden sonra açıklayacağım.

`index.html`:

Burası, sitenizin anasayfasıdır. Domain adınızı aldığınızda, ya da almayıp kullanıcıadı.github.io sayfanıza girdiğinizde karşınıza çıkacak sayfa budur. Dolayısıyla, buraya genellikle, blog yazılarınızın yeniden eskiye sıralaması (adı üstünde, *index*) gibi bir yapı yerleştirirler.

```html
<html>
    <head>
        <title>{ { page.title } }</title>
    </head>
    { % include head.html % }
    <body>
        { % include header.html % }

        { % for post in site.posts % }
        	<a href="{ { post.url } }">
                <h4>{ { post.title } }</h4>
            </a>
        	{ { post.excerpt } }
        	{ { post.date | date: "%d.%m.%Y" } }
        	{ % if post.tags.size > 0 % }
        		{ % if post.tags.size > 1 % }
        			{ { post.tags | sort | join: ", " } }
        		{ % else % }
        			{ { post.tags } }
        		{ % endif % }
        	{ % endif % }
        { % endfor % }

        { % include footer.html % }
    </body>
</html>
```

Böyle bir kod sonucunda, artık anasayfanızda, bugüne kadar post ettiğiniz bütün yazılarınız yeniden eskiye; kısa özetleri, tarihleri ve tag'leri ile birlikte yayınlanırlar. Tabii bu kod parçasını çok temel tuttum, HTML ve CSS özelliklerini hiç yazmadım. Bunlar istenildiği gibi oynanıp şekillenebilirler. İsterseniz, bu sitenin footer'ında, bütün tasarım ve kodlamayı yayımladığımı söylediğim kısma gidip benim sayfalarımın nasıl yazıldığına bakabilirsiniz. Buna yakındırlar.

Includes ve Layouts klasörlerindeki HTML dosylarının ve index'in içinde Liquid programlayarak sitemizin en temel yapısını oluşturduk. Bu üç örnek, bir başlangıç olabilir. Jekyll için hazırlanmış şu şekilde bir [cheatsheet](https://devhints.io/jekyll) de var. Bunların dışında, şöyle birkaç referans linki verebilirim: [bir](https://shopify.github.io/liquid/basics/introduction/), [iki](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers), [üç](https://learn.cloudcannon.com/jekyll/date-formatting/).

### Markdown

Markdown, içerik yazmak için hazırlanmış bir dil, bir *sayfa biçimlendirme biçimi*dir. Jekyll açısından, kullanıcı tarafından kolayca yazılmış bir içeriğin html sayfası içine program aracılığıyla gömülmesi için kullanılır. Kullanımı kolay olduğu gibi, öğrenmesinin zevkli olduğunu söyleyebilirim. Örneğin, bu tabloyu kolayca, onun altındakileri yazarak yaptım.

| Ülke    | Norveç | İsveç     | İzlanda  |
| ------- | ------ | --------- | -------- |
| Başkent | Oslo   | Stockholm | Rekjavik |
| Durum   | Zengin | Zengin    | Zengin   |

```markdown
|Ülke|Norveç|İsveç|İzlanda|
|----|----|----|----|
|Başkent|Oslo|Stockholm|Rekjavik|
|Durum|Zengin|Zengin|Zengin|
```

Hatta, Markdown, kendi içinde Mermaid şema dilini de kullanmamıza izin verdiğinden, şöyle fantastik şeyler de yapabiliyorsunuz:

```mermaid
graph LR;
    A-->B[ben B'yim];
    A-->C((yuvarlak bir C));
    B-->D>çentikli D];
    C-->D;
```

```
graph LR;
    A-->B[ben B'yim];
    A-->C((yuvarlak bir C));
    B-->D>çentikli D];
    C-->D;
```

Tabii daha fazla kullanacaklarınıza gelirsek, yıldızlar (`* * `) arasında kullandığınız şeyler italic, çift yıldız arasında yazdıklarınız (`** **`) bold, bir taneden altı taneye kadar olan kare simgeleri (`#`) büyükten küçüğe başlıklar, `[]()` simgesinde normal parantez içine url'yi koyarken köşeli parantez içine yazılan linkin sayfada görünen sözcüğü... oluyor. Bunların tamamı [şurada](https://www.markdownguide.org/cheat-sheet/). Ya da isterseniz [şuradaki gibi](https://dillinger.io/) online bir editor kullanarak kendiniz de deneyebilirsiniz. (Mermaid içinse [burası](https://mermaidjs.github.io/))

İyi güzel de, yazdığımız markdown sayfasını Jekyll aracılığıyla internette nasıl yayımlayacağım? İşte bu en kolayı. Markdown belgenizin (.md uzantılı belge) en üstüne, hiçbir şey yazmadan önce iki adet --- (üç tire) koymanız, ve arasına, en üstte belirttiğim `_layouts` klasöründen istediğiniz bir html dosyasını referans vermeniz gerekiyor. Şöyle:

```mark
---
layout: default
---
```

Böylece, Jekyll, default.html'in içine yazdığınız { { content } } satırının yerine, sözü geçen markdown belgesinin içindeki her şeyi bütün şeklini şemalini de vererek pat diye koyuyor. 

Başlık, yazıyı temsil eden resim, yazıyı temsilen yazılmış bir cümle, tag'ler gibi şeyler içinse aşağıdaki örneği yazabilirim. Hatta bu, tam olarak üzerinde bulunduğunuz sayfanın front matter'ı olsun (o üç çizgiler içine yazılan şeye front matter denir).

Tarih ise, Jekyll tarafından markdown belgenizin kendisinin isminden çekiliyor. Bu da, en yukarıda hazırladığom tablo için açıklamam gereken son klasörü ilgilendiriyor: `_posts`. Bu klasörün içine, belgelerinizi tam olarak **yıl-ay-gün-başlık.md** şeklinde kaydetmelisiniz. Yoksa Jekyll çalışmaz. 

```
2018-12-27-jekyll-2.md
```

## . . .

En son da, `_config.yml` dosyanızın içine, sitenin Türkçe karakter kullanacağını belirtmelisiniz. Aslında bu dosyanın da [bir sürü kullanım şekli](https://circleci.com/docs/2.0/sample-config/) var, ama bu yazı bir Jekyll'e giriş yazısı olduğundan, güzel çalışabilecek en temel kodları kullandım.

```
encoding: UTF-8
```

Böylece, sitemiz de, yazımız da, bu yazının sitemizin anasayfasındaki temsili ve linki de hazır. 

Bu yazıda, CSS'ten, javascriptten hiç bahsetmedim. Bunları örneğin bir `assets` klasörü oluşturarak, layout'larınızdan birine bağlayabilirsiniz (temel HTML bilgisi yani, `<link rel="..." href="..."`'ten bahsediyorum').

Bu yazıyı okuyup da sorularınız olduğunda, iletişim sayfamı kullanmaktan çekinmeyebilirsiniz.