---
setup: |
  import Layout from '../../layouts/BlogPostLayout.astro';
title: ChatGPT's - Simple HTTP Server (clojure)
description: Creating a simple http server that host a directory in clojure
date: 2023-01-15
author: "Gold Ayan"
category: "Coding"
tags:
  - clojure
  - chatgpt
---

when i want to transfer a file in a network, I normal use the following command to host a simple http server in python that host the current directory
```python
python3 -m http.server 8888
```
Here we are using the http.server module and telling it to run in _8888_ port    

One of my junior developer uses chatgpt at work to solve some problems

I was impressed very much with the relevant answer, I logged in yesterday and tried bunch of other things and i thought
> HMM why not ask the ChatGPT to create a simple HTTP server in clojure
I started asking ton of question and the answer is just pouring out, It's like raining but you cann't harvest all the water. Time to harvest some answers.

I started connecting the pieces together and voila, HTTP server that can serve a directory in clojure. It makes few mistakes though not biggy    

Let's discuss the code piece by piece

We need HTTP server lib first so i choosed `http-kit`.

deps.edn
```
{:deps {http-kit/http-kit {:mvn/version "2.7.0-alpha1"}}}
```

Full code: https://github.com/ThangaAyyanar/Learning-Adventure/blob/master/Clojure/simple-http-server.clj


Let's get straight into the coding

```clojure
(ns simple-http-server
  (:require [org.httpkit.server :as http]
            [clojure.java.io :as io]
            [clojure.string :as string]))
```
here we required 
- `java.io` to reading files and folder
- `string` split and join the string
- Next, Setting up the http sever
```clojure
(defonce server (atom nil))

(defn stop-server []
  (when-not (nil? @server)
    ;; graceful shutdown: wait 100ms for existing requests to be finished
    ;; :timeout is optional, when no timeout, stop immediately
    (@server :timeout 100)
    (reset! server nil)))

(defn -main [& args]
  (reset! server (http/run-server #'handler {:port 8080})))
```
- Starting and stoping the http server code
- Below we have the handler function that does the routing and downloading file logics
```clojure
(defn get-path [uri]
    (if (= uri "/") dir-path (str dir-path uri)))

(def error-response {:status 404 :body "404 Not Found"})

(defn file-download-response [file-path]
  {:status 200
   :headers {"Content-Type" "application/octet-stream"
             "Content-Disposition" (str "attachment; filename="(.getName file-path))}
   :body (io/input-stream file-path)})

(defn folder-html-response [uri path]
  {:status 200
   :headers {"Content-Type" "text/html"}
   :body (contents uri path)})

(defn handler [request]
  (let [uri (or (:uri request) "/")
        path (get-path uri)
        file-path (io/file path)]
    (if file-path
      (cond
        (.isFile file-path) (file-download-response file-path)
        (.isDirectory file-path) (folder-html-response uri path)
        :else error-response
        )
      error-response)))
```
- one of the core function of this program is getting all the files and directory
```clojure
(defn list-folders
  ([path] (list-folders "/" path))
  ([uri path]
    (->> (io/file path)
        (.listFiles)
        (map #(dirs-to-string uri %))
        (string/join)
        )))
```
- Here we used multi arity function.
- First we will convert the path Eg: `/User/Ayan/Temp` into file object.
- Get all the files and folders using `.listFiles`
- Convert it to strings using dirs-to-string function
```clojure
defn folder-html [uri name]
 (str "<li><a href='" (str uri name) "'>" (str name "/") "</a> </li>"))

(defn file-html [uri name]
 (str "<li><a href='" (str uri name) "' download>" (str name) "</a> </li>"))
 
(defn dirs-to-string [uri file]
  (let [name (.getName file)
        new-uri (if (= uri "/") uri (str uri "/"))]
    (cond
      (.isDirectory file) (folder-html new-uri name)
      (.isFile file) (file-html new-uri name)
      :else "")))
```
- We use `.getName` function to get the name of the file or folder.
- Then construct a html string out of it.
```clojure
(defn prepend-up-dir [uri]
  (if (= uri "/") nil
      (let [prev-folder (string/join "/" (pop (string/split uri #"/")))]
        (str "<li><a href='" (if (empty? prev-folder) "/" prev-folder) "'>..</a></li>"))))
```
- Also added a link to traverse back to previous folder if we move into other folder other than root.
- Finally generating overall html
```clojure
(defn generate-html
  [title uri items]
  (let [title-html (str "<h1>" title "</h1>")
        uri-html  (str "<b>URI:</b>" uri)
        prev-dir (or (prepend-up-dir uri) "")
        contents (str "<hr/><ul>" prev-dir items "</ul><hr/>")]
     (str title-html uri-html contents)))

(defn contents [uri path]
  (let [folders (list-folders uri path)]
    (generate-html "Simple HTTP Server" uri folders)))
```
- `generate-html` function generate page for the browser
- I know, I have not included the boiler plate html code like html, head and body tags.
- That is a exercise for you :p


### Would you like to go further, Try few things below
- Run the above script in babashka
- How to add simple authentication token ?
    - Simple Token in header
	- Authentication using jwt token
- Restrict maximum concurrent downloads
- My future self crazy idea's

### Resources
- ChatGPT
- Clojure Examples
- Amazing clojure docs website
- Last but not least, Stackoverflow
