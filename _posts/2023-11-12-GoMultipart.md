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


multipart
====================

## multipart 설명
### multipart
Go 언어에서는 mime/multipart 패키지를 사용하여 multipart/form-data 형식의 요청을 생성할 수 있음. multipart/form-data는 파일 업로드와 같은 이진 데이터를 전송하기 위해 사용되며, 일반적으로 웹 양식에서 파일을 업로드할 때 사용됨.

  - 일반적으로 아래 코드와 같이 "mime/multipart" 패키지를 이용하여 사용하여 간편하게 사용
multipart.Writer 타입을 제공, multipart.Writer는 io.Writer 인터페이스를 구현, 이를 사용하여 multipart 요청의 각 part를 작성

## 기본형 ( local file 업로드, 주석으로 설명)
```
package main

import (
	"bytes"
	"fmt"
	"io"
	"mime/multipart"
	"net/http"
	"os"
	"path/filepath"
    "net/textproto"
)

func main() {
	// Open the file to upload
	path = "/path/to/filepath.txt"      // file path
	metadata := map[string]string{          //server에서 요구 파라미터
		"param1":       "param1 data",
		"param2":      "param2 data",
	}

    file, err := os.Open(path)  //file 정보 받아오기
	if err != nil {
		return nil, err
	}
	defer file.Close()

    //mutipart 구성 시작
	body := &bytes.Buffer{}     // request body 생성
	writer := multipart.NewWriter(body)  // multipart 생성
	part, err := writer.CreateFormFile(paramName, filepath.Base(path))
	if err != nil {
		return nil, err
	}
	_, err = io.Copy(part, file) // file > part 데이터 복사

	//server 설정에 따라 변경
    for key, val := range metadata {
		_ = writer.WriteField(key, val)
	}
    //textproto.MIMEHeader{} 사용해서 multipart Content-Disposition, Content-Type 변경 가능 아래 예시 일반적으로  Content-Disposition 는 from-data 사용, 다운로드가 필요할 시 attachment 사용
    //metadataHeader := textproto.MIMEHeader{}
	//metadataHeader.Set("Content-Disposition", "form-data; name=\"flag\"")
	//part, _ := mwriter.CreatePart(metadataHeader)
	//part.Write([]byte(metadata.param1))

	err = writer.Close()
	if err != nil {
		return nil, err
	}
    // mutipart 설정 끝

	req, err := http.NewRequest("POST", "https://path/to/url", body)    // 요청 request URL, method setting, body(위에서 설정한) ,bytes.NewReader(body.Bytes()) 형태로 넘겨주기도 함 서버 설정에 따라 다름 
	req.Header.Set("Content-Type", writer.FormDataContentType())    // http request header setting

	if err != nil {
		fmt.Println(err)
	}
	client := &http.Client{
        Timeout: 3 * time.Minute,   // server 연결 timeout 설정 가능
    }
	resp, err := client.Do(request) // server에 전송
	if err != nil {
		fmt.Println(err)
        return nil, err
	}

	// response body 처리 방법1
    body := &bytes.Buffer{}
	_, err := body.ReadFrom(resp.Body)
    if err != nil {
		fmt.Println(err)
        return nil, err
	}

    // response body 처리 방법2
    // type Resp_obj struct {
	//	Code int `json: "code"`
    //  Message String `json: "message"`
    // }
    // resp_obj := Resp_obj{}
    // json.Unmarshal(respBody, &resp_obj)

    resp.Body.Close()

	fmt.Println(resp.StatusCode)
	fmt.Println(resp.Header)
	fmt.Println(body)   // 방법 1 사용시
    //fmt.println(resp-obj.Code) // 방법 2 사용시
    //fmt.println(resp-obj.message)
	}
}
```
