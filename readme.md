# Örneklerde Kullanılacak olan json formatı :

```json
{
	"projectName": "Project Name",
	"navbarMenu": [
		{
			"link": "index.html",
			"name": "Anasayfa"
		},
		{
			"link": "about.html",
			"name": "Hakkımızda",
			"subMenu": [
				{
					"link": "about.html#vision",
					"name": "Vizyonumuz"
				},
				{
					"link": "about.html#mission",
					"name": "Misyonumuz"
				},
				{
					"link": "about.html#History",
					"name": "Tarihçemiz"
				},
				{
					"link": "about.html#certificate",
					"name": "Sertifikalarımız"
				}	
			]
		},
		{
			"link": "contact.php",
			"name": "İletişim"
		}
	],
	"pages": {
		"home": {
			"pageTheme": "dark",
			"pageName": "Anasayfa",
		},
		"about": {
		"pageName": "Hakkımızda",
			"breadcrumbs": {
			"tree": [
					{
						"name": "Anasayfa",
						"link": "index.html"
					},
					{
						"name": "Hakkımızda",
						"link": "about.html"
					}
				]
			}
		}
	}
}
```

# Temeller 
## Dışardan Template Çekmek İçin

- Bu syntax, dışarıdan bir Handlebars template'i çekmek için kullanılır. `"Dosya_Yolu"` kısmına çekmek istediğiniz template'in dosya yolunu belirtmelisiniz.
#### Syntax :
```handlebars
{{> "Dosya_Yolu"}}
```

#### Örnek :
```handlebars
{{> "layout/footer.hbs"}}
```
-----
## Dışardan Argüman ile Template Çekmek İçin

 - Bu syntax, dışarıdan bir Handlebars template'i çekerken argüman eklemek için kullanılır. `"Dosya_Yolu"` kısmına çekmek istediğiniz template'in dosya yolunu, `argumanAdı` kısmına ise argümanın adını belirtmelisiniz.
#### Syntax : 

```handlebars
{{> "Dosya_Yolu" argumanAdı="Argüman"}}
```
#### Örnek : 
```handlebars
{{> "layout/footer.hbs" data="argüman"}}
```
----
## Gelen Argümanı Template İçinde Kullanmak :

 - Bu syntax, bir argümanı template içinde kullanmak için kullanılır. `argümanAdı` kısmına kullanmak istediğiniz argümanın adını belirtmelisiniz.
#### Syntax : 
```handlebars
	<h1>{{argümanAdı}}</h1>
```

#### Örnek : 
```handlebars
	<h1>{{data}}</h1>
```

## Attributes Doldurmak:

 - Bu syntax, bir HTML elementinin attribute'lerini doldurmak için kullanılır. `{{değer}}` kısmına, ilgili attribute'leri elde etmek için kullanılan Handlebars ifadelerini eklemeniz gerekir.
#### Syntax:
```handlebars
	<a href="{{değer}}" class="{{değer}}" id="{{değer}}"></a>
```
#### Örnek:
```handlebars
	<a href="{{this.link}}">{{this.name}}</a>
```
## If ve Else Sorgusu : 

 - Bu örnek, `data.breadcrumbs.desc` değerini kontrol eder. Eğer bu değer true ise, ilk `if` bloğu içindeki `<p>` elementi gösterilir. Eğer değer false ise, ikinci `else` bloğu içindeki `<p>` elementi gösterilir.
#### Syntax : 
```handlebars
	{{#if sorgu}}
		Doğru İse Göster
	{{/if}}

	{{#if sorgu}}
		Doğru ise Göster
	{{else}}
		Değil ise Göster
	{{/if}}
```

#### Örnek : 
```handlebars
	{{#if data.breadcrumbs.desc}}
		<p>{{data.breadcrumbs.desc}}</p>
	{{/if}}

	{{#if data.breadcrumbs.desc}}
		<p>{{data.breadcrumbs.desc}}</p>
	{{else}}
		<p>Açıklama Bulunamadı</p>
	{{/if}}
```

## Bir Dizin Obje'yi Dönmek İçin :

 - Bu syntax, bir dizinin içindeki her bir öğeyi döngüye almak ve içeriğini göstermek için kullanılır. `ArrayAdı` kısmına döngüye almak istediğiniz dizinin adını belirtmelisiniz. `dönülenObje` kısmında ise her döngü adımında göstermek istediğiniz içeriği belirtirsiniz.
 - Bu örnek, `data.breadcrumbs.tree` dizisinin her bir öğesini döngüye alır. Her döngü adımında, `<a>` etiketi içinde öğenin `link` ve `name` özelliklerini kullanarak bir liste elemanı oluşturur.
#### Syntax : 

```handlebars
	{{#each ArrayAdı}}
		{{dönülenObje}}
	{{/each}}
```

####  Örnek :

```handlebars
	<ul>
		{{#each data.breadcrumbs.tree}}
			<li>
				<a href="{{this.link}}">{{this.name}}</a>
			</li>
		{{/each}}
	</ul>
```


## Döngü İçinde Tur Yakalamak :

 - Bu syntax, bir döngü içinde bulunan her bir öğe için belirli durumları yakalamak ve özel işlemler yapmak için kullanılır. `array` kısmına döngüye alınan dizinin adını belirtmelisiniz.
	- `@first`: Döngünün ilk öğesindeyken true değeri döndürür.
	- `@last`: Döngünün son öğesindeyken true değeri döndürür.
	- `@odd`: Döngü içindeki tek sıra numarasına sahip öğeler için true değeri döndürür.
	- `@even`: Döngü içindeki çift sıra numarasına sahip öğeler için true değeri döndürür.
	- `@index`: Bulunduğunuz öğenin index numarasını döndürür.
- Bu örnek, `this.subMenu` dizisinin her bir öğesini döngüye alır. Her bir öğe için, `@first` ile ilk öğe kontrol edilir ve eğer ilk öğe ise `active` class'ı eklenir. Ayrıca, `@index` ile öğenin index numarası alınır ve bu değer bir animasyon gecikme süresi olarak kullanılır.
#### Syntax :
```handlebars
	{{#each array}}
		{{#if @first}}
		{{/if}}
		{{#if @last}}
		{{/if}}
		{{#if @odd}}
		{{/if}}
		{{#if @even}}
		{{/if}}
		{{@index}}
	{{/each}}
```

#### Örnek : 

```handlebars
{{#each this.subMenu}}
	<li class="{{#if @first}active{{/if}}" style="animation-delay: .{{@index}}s;">
	{{@index}}
		<a href="{{ this.link }}">
			{{this.name}}
		</a>
	</li>
{{/each}}
```

# data.json'da ki değerler ile yapılmış bir menü örneği :

```html
<header>
	<ul>
		{{#each menuExample}}
			<a href="{{this.link}}">{{this.name}}</a>
			{{#if this.subMenu}}
				<ul>
					{{#each this.subMenu}}
						<li class="{{#if @first}active{{/if}}" style="animation-delay: .{{@index}}s;">
							<h1>{{@key}}'. menü</h1>
							<a href="{{ this.link }}">
								{{this.name}}
							</a>
						</li>
					{{/each}}
				</ul>
			{{/if}}
		{{/each}}
	</ul>
</header>
```

# Generic Argümanlı Breadcrumb Örneği :

## breadcrumb.hbs
```html
<div>
	<h1>
	{{data.pageName}}
	</h1>
	
	{{#if data.breadcrumbs.tree}}
		<ul>
			{{#each data.breadcrumbs.tree}}
				<li>
					<a href="{{this.link}}">{{this.name}}</a>
				</li>
			{{/each}}
		</ul>
	{{/if}}
	
	{{#if data.breadcrumbs.desc}}
		<p>{{data.breadcrumbs.desc}}</p>
	{{else}}
		<p>Açıklama Bulunamadı</p>
	{{/if}}
</div>
```
## about.hbs
```handlebars
	{{> layout/breadcrumb.hbs data=pages.about}}
```
