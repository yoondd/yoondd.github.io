```
interface Device {  
    Laptop: number;  
    Phone: number;  
    Tablet: number;  
    Pc: number;  
}  
  
const getDevices = async (): Promise<Device> => {  
    const res = await fetch("http://localhost:3000/device.json");  
    const data = await res.json();  
    return data;  
}  
  
getDevices().then(data => {  
    console.log(`Laptop: ${data.Laptop}`);  
    console.log(`Phone: ${data.Phone}`);  
    console.log(`Tablet: ${data.Tablet}`);  
    console.log(`Pc: ${data.Pc}`);  
})
```