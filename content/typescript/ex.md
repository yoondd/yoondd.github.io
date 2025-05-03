```ts
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


---

json데이터가 변경되어 다시 작업한 결과 ,

```ts
interface Device {  
    id: number;  
    name: string;  
    price: number;  
}  
  
const getDevices = async (): Promise<Device[]> => {  
    const res = await fetch("http://localhost:3000/device2.json");  
    const newData = await res.json();  
    return newData;  
}  
  
getDevices().then(data => {  
    for( let item in data ){  
        console.log(data[item].name + ": " + data[item].price);  
    }s  
})
```