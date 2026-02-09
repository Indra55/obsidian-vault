### Streams & Buffers 

- A **Buffer** is a fixed-size chunk of raw binary data.
    
    - Useful when dealing with files, network packets, or any binary protocol.
        
    - Example: `Buffer.from('hello')` → `<Buffer 68 65 6c 6c 6f>`
        
- A **Stream** is a continuous flow of data, broken into multiple buffers.
    
    - Enables processing **large files or incoming data** piece-by-piece.
        
    - Streams are **asynchronous**, **event-driven**, and memory-efficient.
        
    - Common events:
        
        - `data` → emits each buffer chunk
            
        - `end` → signals completion
            
        - `error` → handles failures
            

> **TL;DR**:  
> 🧱 **Buffer** = a chunk of binary data  
> 💧 **Stream** = async flow of Buffers over time (ideal for large or continuous data)

---

```js
const fs = require('fs');

// Create a readable stream from a file
const stream = fs.createReadStream('example.txt');

stream.on('data', (chunk) => {
  console.log('🔹 Buffer chunk received:', chunk);
  console.log('🔸 As string:', chunk.toString());
});

stream.on('end', () => {
  console.log('✅ Finished reading the file.');
});
```

---

### 🧾 Example Output (Assume `example.txt` contains "Hello Hitanshu!"):

```
🔹 Buffer chunk received: <Buffer 48 65 6c 6c 6f 20 48 69 74 61 6e 73 68 75 21>
🔸 As string: Hello Hitanshu!
✅ Finished reading the file.
```

> If the file is very large, multiple `chunk` events will be emitted, each containing a part of the file as a `Buffer`.

---

### ✅ Summary

- `chunk` is a `Buffer` containing raw bytes.
    
- `chunk.toString()` converts it to human-readable text.
    
- The file is read **efficiently**, without loading the entire thing into memory.
    

Let me know if you want a version that reads user input or streams from the network instead!