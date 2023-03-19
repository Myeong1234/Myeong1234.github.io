---
title: "Golang multipart 정리"
toc: true
toc_sticky: true
excerpt_separator: "<!--more-->"
author_profile: true
sidebar:
  nav: "main"
categories:
  - Post Formats
tags:
   - Golang
---

# multipart
Go 언어에서는 mime/multipart 패키지를 사용하여 multipart/form-data 형식의 요청을 생성할 수 있음. multipart/form-data는 파일 업로드와 같은 이진 데이터를 전송하기 위해 사용되며, 일반적으로 웹 양식에서 파일을 업로드할 때 사용됨.

  - 일반적으로 아래 코드와 같이 "mime/multipart" 패키지를 이용하여 사용하여 간편하게 사용
multipart.Writer 타입을 제공, multipart.Writer는 io.Writer 인터페이스를 구현, 이를 사용하여 multipart 요청의 각 part를 작성

Go 예시
```python
package main

import (
    "bytes"
    "mime/multipart"
    "net/http"
    "os"
)

func main() {
    // Open the file to upload
    file, err := os.Open("/path/to/file")
    if err != nil {
        panic(err)
    }
    defer file.Close()

    // Create a new multipart writer
    var buf bytes.Buffer
    writer := multipart.NewWriter(&buf)

    // Create a new file field
    fileField, err := writer.CreateFormFile("file", "example.txt")
    if err != nil {
        panic(err)
    }

    // Copy the file contents to the file field
    _, err = io.Copy(fileField, file)
    if err != nil {
        panic(err)
    }

    // Close the multipart writer
    writer.Close()

    // Create a new HTTP request with the multipart content
    req, _ := http.NewRequest("POST", "http://example.com/upload", &buf)

    // Set the content type header
    req.Header.Set("Content-Type", writer.FormDataContentType())

    // Send the request using an http client
    client := &http.Client{}
    res, _ := client.Do(req)

    // Handle the response
    // ...
}
```


# 예외사항
CreateFormFile으로 작성해 주게되면 내부의 Content-type이 application/octet-stream으로 고정되며, 바꾸고 싶다면 CreatePart를 사용하여 직접 작성 해줄 필요가 있음 (예시 1번 Header.Set 이된다고 나와있는데 사용이 안되었음 2번 방법으로 성공)

Go 코드 예시 1
```python
func main() {
    // Create a new multipart writer
    var buf bytes.Buffer
    writer := multipart.NewWriter(&buf)



    func main() {
    // Create a new multipart writer
    var buf bytes.Buffer
    writer := multipart.NewWriter(&buf)

    // Create a new form field
    field, err := writer.CreateFormField("field1")
    if err != nil {
        panic(err)
    }

    // Set the Content-Disposition header
    field.Header.Set("Content-Disposition", `form-data; name="field1"`)
    field.Header.Set("content-Type","application/json") // 헤더 추가 변경 가능

    // Write the form field value
    field.Write([]byte("value1"))

    // Close the multipart writer
    writer.Close()
    
    // Create a new HTTP request with the multipart content
    req, _ := http.NewRequest("POST", "http://example.com/upload", &buf)

    // Set the content type header
    req.Header.Set("Content-Type", writer.FormDataContentType())

    // Send the request using an http client
    client := &http.Client{}
    res, _ := client.Do(req)

    // Handle the response
    // ...
}

```


예시 2

```python
func main() {
    // Create a new multipart writer
    var buf bytes.Buffer
    writer := multipart.NewWriter(&buf)

    // Create a new part
    part, err := writer.CreatePart(map[string][]string{
        "Content-Type": {"text/plain"},
        "Content-Disposition": {`form-data; name="text"`},  // 설정 필수
    })
    if err != nil {
        panic(err)
    }

    // Write data to the part
    _, err = part.Write([]byte("Hello, World!"))
    if err != nil {
        panic(err)
    }

    // Close the multipart writer
    writer.Close()
        // Create a new HTTP request with the multipart content
    req, _ := http.NewRequest("POST", "http://example.com/upload", &buf)

    // Set the content type header
    req.Header.Set("Content-Type", writer.FormDataContentType())

    // Send the request using an http client
    client := &http.Client{}
    res, _ := client.Do(req)

    // Handle the response
    // ...

```