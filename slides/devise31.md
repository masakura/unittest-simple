こういうのはテストしにくい!

```csharp
public bool IsInRange
{
    get {
        var now = DateTime.Now;
        return StartDateTime <= now && now < EndDateTime;
    }
}
```
