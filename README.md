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

