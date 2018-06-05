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
Proces zamiany obiektu na obiekty typu JSON

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

