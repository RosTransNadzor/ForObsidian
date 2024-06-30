[[Entity Framework]]

- Пример использования Enum(Mysql)
```cs
public class Order  
{  
    public int OrderId { get; set; }  
    public DateTime DateOrdered { get; set; }  
    public OrderStatus Status { get; set; }  
    // refs  
    public int CustomerId { get; set; }  
    // links   
public ICollection<LineItem> LineItems { get; set; } = null!;  
}  
// в базе данных все значения заменяться на int
// Unpaid - 0(по умолчанию),Collecting -1 , и т.д.
public enum OrderStatus  
{  
    UnPaid,  
    Collecting,  
    Delivering  
}
```

![[enums.png]]