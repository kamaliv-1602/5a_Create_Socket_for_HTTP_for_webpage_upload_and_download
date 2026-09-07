# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
```
page.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Socket Upload Test</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f9;
            color: #333;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            margin: 0;
        }
        .card {
            background: white;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            text-align: center;
        }
        h1 { color: #007bff; }
    </style>
</head>
<body>
    <div class="card">
        <h1>Upload Successful!</h1>
        <p>This HTML file was uploaded and served via custom Python raw sockets.</p>
    </div>
</body>
</html>

http_socket.py

import socket, threading, webbrowser

def server():
    s = socket.socket()
    s.bind(("localhost", 8080))
    s.listen(1)

    while True:
        c, _ = s.accept()
        req = c.recv(10000)

        if req.startswith(b"POST"):
            data = req.split(b"\r\n\r\n", 1)[1]
            open("uploaded.txt", "wb").write(data)
            c.sendall(b"HTTP/1.1 200 OK\r\n\r\nUpload successful")

        elif req.startswith(b"GET"):
            data = open("uploaded.txt", "rb").read()
            c.sendall(b"HTTP/1.1 200 OK\r\n\r\n" + data)

        c.close()

threading.Thread(target=server, daemon=True).start()
webbrowser.open("http://localhost:8080")
# Upload
data = open("page.html", "rb").read()
s = socket.socket()
s.connect(("localhost", 8080))
req = b"POST /upload HTTP/1.1\r\nContent-Length: " + str(len(data)).encode() + b"\r\n\r\n" + data
s.sendall(req)
print(s.recv(4096).decode())
s.close()

# Download
s = socket.socket()
s.connect(("localhost", 8080))
s.sendall(b"GET /uploaded.txt HTTP/1.1\r\n\r\n")
response = s.recv(10000)
open("downloaded.txt", "wb").write(response.split(b"\r\n\r\n", 1)[1])
s.close()

print("File downloaded successfully.")
```
## OUTPUT

<img width="1917" height="1008" alt="Screenshot 2026-09-07 121409" src="https://github.com/user-attachments/assets/b3f564fc-cb6a-48e0-9918-367c6f6ce68a" />
<img width="1917" height="1012" alt="Screenshot 2026-09-07 120418" src="https://github.com/user-attachments/assets/cb5ab000-8c87-46d8-a25d-b784dc8177d2" />






## Result
Thus the socket for HTTP for web page upload and download created and Executed
