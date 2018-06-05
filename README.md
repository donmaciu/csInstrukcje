# C#

## JSON Serializacja, deserializacja

### Obiekty

JSON:
```json
{
  "Name" : "Jakaś nazwa",
  "Description": "Jakiś opis"
}
```

Klasa C#
```c#
[DataContract]  
class BlogSite  
{  
    [DataMember]  
    public string Name { get; set; }  
  
    [DataMember]  
    public string Description { get; set; }  
}  
```

### Serializacja
Proces zamiany obiektu c# na ciąg typu JSON

```c#
// Tworzenie obiektu do serializacji
BlogSite bsObj = new BlogSite()  
{  
    Name = "C-sharpcorner",  
    Description = "Share Knowledge"  
};  

// Utworzenie obiektu narzędzia serializującego ze wskazaniem na typ serializowany (tutaj BlogSite)
DataContractJsonSerializer js = new DataContractJsonSerializer(typeof(BlogSite));
// Utworzenie instancji strumienia MemoryStream
MemoryStream msObj = new MemoryStream();
// metoda obiektu js serializuje obiekt bsObj do strumienia msObj
js.WriteObject(msObj, bsObj);
// ustawienie strumienia w pozycji 0 (na początku), żeby można było wykonać odczyt
msObj.Position = 0;

// utworzenie obiektu czytającego strumień
StreamReader sr = new StreamReader(msObj);  

// Odczyt strumienia do zmiennej typu string
// Poniżej uzyskany ciąg znaków:
// "{\"Description\":\"Share Knowledge\",\"Name\":\"C-sharpcorner\"}"  
string json = sr.ReadToEnd();  

// Bardzo ważne w przypadku strumieni - zawsze należy je zamknąć po zakończeniu korzystania z nich
sr.Close();  
msObj.Close();  
```

### Deserializacja
Proces zamiany ciągu znaków w formacie json na obiekt c#

```c#
// zdefiniowanie zmiennej przechowującej ciąg JSON
string json = "{\"Description\":\"Share Knowledge\",\"Name\":\"C-sharpcorner\"}";  

// Blok using, w którym został zdefiniowany nowy strumień.
// Strumień jest tworzony na podstawie bajtów uzyskanych za pomocą funkcji Encoding.Unicode.GetBytes
// WAŻNE: bloki using automatycznie zamykają strumienie po zamknięciu bloku
using (var ms = new MemoryStream(Encoding.Unicode.GetBytes(json)))  
{  
   // Utworzenie obiektu deserializującego z określeniem typu obiektu, do którego będzie wykonana deserializacja  
   DataContractJsonSerializer deserializer = new DataContractJsonSerializer(typeof(BlogSite));
   // deserializer.ReadObject czyta ciąg JSON ze strumienia i konwertuje go na obiekt c#. Należy jednak tak uzyskany obiekt
   // rzutować na typ, który chce się uzyskać - w tym przypadku jest to BlogSite.
   BlogSite bsObj2 = (BlogSite)deserializer.ReadObject(ms);  
}  
```

## Pobieranie tekstu (JSONa) z adresu URL z poziomu kodu c#
Najprostszym sposobem uzyskania ciągu znaków JSON za pomocą metody GET z adresu internetowego jest skorzystanie z klasy WebClient

```c#
// Utworzenie obiektu WebClient
using (WebClient wc = new WebClient())
{
  // DownloadString pobiera zawartość dostępną pod linkiem i zapisuje ją w zmiennej json
   string json = wc.DownloadString("https://jakis.adres.pl");
}
```

Jeśli należy użyć dodatkowych nagłówków przy pobieraniu danych można je zdefiniować w poniższy sposób

```c#
// wc jest obiektem klasy WebClient
wc.Headers.Add("Bearer", "jakisToken");
```

Jeśli chcemy wykonać zapytanie zapomocą metody POST

```c#
using (WebClient wc = new WebClient())
{
    // Ustawienie nagłówków zawierających opis przesyłanej treści
    wc.Headers.Add("Content-Type","application/x-www-form-urlencoded");
    
    // uzyskanie tablicy bajtowej na podstawie ciągu znaków w zmiennej postData
    byte[] byteArray = Encoding.Unicode.GetBytes(postData);
    // funkcja Upload data przyjmuje jako argumenty adres URL, nazwę metody (tutaj POST), oraz uzyskaną powyżej tablicę
    // uzyskujemy w ten sposób zawartość zwracanej witryny/pliku w formie tablicy bajtów
    byte[] byteResult= wc.UploadData("https://jakis.adres.pl","POST",byteArray);
    // zamiana tablicy bajtów na zmienną typu string
    string zeStrony = Encoding.Unicode.GetString(byteResult);
}
```

