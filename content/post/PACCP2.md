+++
author = "Hugo Authors"
title = "ProLUG Admin Course Capstone Project Stage 2 🐧"
date = "2024-09-27"
description = "ProLUG Admin Course Capstone Project Week 2"
tags = [
  "Tech", "Linux", "Administration", "Engineering", "ProLUG Course", "CapstoneProject"
]
categories = [
    "Learning",
]
series = ["Learning"]

+++

<!--more-->

# Intro 👋

I’ve made significant progress with the Capstone Project, the project has expanded to include building a full website.

---

## Planning

### Integration

It seemed silly to develop such a complex mechanism that would later require integration into an unknown larger structure.

### No site to begin with

After asking Scott about it, I learned that there has never been a ProLUG website and that he has been meaning to get something up.

### Naturally

It seemed like taking on the entire website project was a good course of action. That way I would be able to decide the layout, functionality and logic all while learning new tools and techniques.

## Technology

### Going with GO

![Go Logo x Deal with it](https://trevorsmale.github.io/techblog/images/PACCP2/godeal.png)

The website uses Go on the backend to serve templates and for back-end logic. While I’ve used Go for template servers before, incorporating templated components similar to Vue.js is new to me. 

#### Here’s why Go excels for this type of project:

- The codebase remains clean, minimal, and easy to understand.
- The dependency system is simple, with no clutter of unnecessary packages.
- It ensures memory safety.
- Go’s HTTP server protects against injection, MITM, side-channel, and other common attacks.
- HTML templating allows for modularity similar to Vue.js.
- Garbage collection ensures long-term running without memory issues.
- The project can be easily containerized or built into a single binary.
- The Go module system (go.mod) ensures future stability through version control.

### Storing Data

Initially, I was looking at SQLite for storing key-value pairs. However, after considering usability, I switched to a simple CSV file.

### Why CSV

The stored data must be easily manipulated by the administrator in a human readable format. With CSV, Scott would be able to amend or edit certificate holders at a glance.

### Operationally

This method is both secure and integral

### How is it secure?

As stated before GO can securely transmit backend data without many common attacks. Additionally using GIT diff feature would ensure alterations are spotted should a compromise be made.

---

## The Structure

### Certificate Verifier

#### Pulling from this test CSV file:

```csv
Johny,Exampleseed,ProLUG Admin Course,2024-11-15,f078b6c4f26a2fae59d50fdb7c761a7f9523d240b2c18b332aac11e0
Kate,Testinger,ProLUG Admin Course,2024-11-15,a523e6d1ece5bb2758f71993ba1a460024dcd10243fa17654af90257
Het, Tanis, ProLUG Admin Course,2015-11-02,a523e6d1ece5bb2758f71993ba1a460024dcd10243fa17324af90257
```

#### Using this logic:

```go
// Certificate verification handler
// This function is the HTTP handler responsible for verifying certificates.
// It is triggered when a user accesses the relevant URL and checks for a certificate based on a hash parameter.
func certHandler(w http.ResponseWriter, r *http.Request) {

    // Extract hash value from the URL query parameters.
    // The hash is used to identify the certificate to verify.
    hash := r.URL.Query().Get("hash")

    // If no hash is provided in the query, return a "Bad Request" error.
    if hash == "" {
        // Send an HTTP error response indicating that the hash is missing.
        http.Error(w, "Hash not provided", http.StatusBadRequest)
        return // Exit the handler function since no hash is available.
    }

    // Load certificates from a CSV file.
    // This function attempts to retrieve all certificates stored in the "certificates.csv" file.
    certificates, err := LoadCertificates("certificates.csv")
    
    // If there's an error loading the certificates (e.g., file not found or corrupted), log the error.
    if err != nil {
        // Log the error message to the server logs and return a "Server Error" to the client.
        log.Printf("Error loading certificates: %v", err)
        http.Error(w, "Server Error", http.StatusInternalServerError)
        return // Exit the handler function due to the error.
    }

    // Initialize a pointer to a Certificate struct.
    // This will hold the matching certificate if a valid hash is found.
    var cert *Certificate

    // Iterate over all loaded certificates.
    // Check each certificate's hash to see if it matches the one provided in the query.
    for _, c := range certificates {
        if c.Hash == hash {
            // If a matching certificate is found, assign it to the 'cert' pointer and break the loop.
            cert = &c
            break
        }
    }

    // Render the result page using the template engine.
    // Pass the certificate (or nil if not found) to the template "result.html" for display.
    err = templates.ExecuteTemplate(w, "result.html", map[string]interface{}{
        "Certificate": cert, // Include the certificate as part of the template data.
    })

    // If there's an error rendering the page (e.g., template not found or syntax error), log the issue.
    if err != nil {
        // Log the error message to the server logs and return an "Internal Server Error" to the client.
        log.Printf("Error rendering verification page: %v", err)
        http.Error(w, "Internal Server Error", http.StatusInternalServerError)
    }
}
```

```GO
package main

// Certificate represents a learning certificate with associated data.
type Certificate struct {
	FirstName     string
	LastName      string
	CertType      string
	DateCompleted string
	Hash          string
}
```
#### The prompt

![ProLUG Verify Step 1](https://trevorsmale.github.io/techblog/images/Sep22site/shot5.png)

#### Inputting a UID hash string

![ProLUG Verify Step 2](https://trevorsmale.github.io/techblog/images/Sep22site/shot6.png)

#### Matching string results in a info retrieval 

![ProLUG Verify Step 3](https://trevorsmale.github.io/techblog/images/Sep22site/shot7.png)

#### An unrecognized string results in an error message

![ProLUG Verify Step 4](https://trevorsmale.github.io/techblog/images/Sep22site/shot8.png)


### Current Site 💄

After getting a working verification system worked out, I moved on to creating a broader website. 

#### Styling

I like sites with a clean, easy to use and accessible appearance. I have a solid understanding of graphic design for accessibility and wanted to put that into practice here. So the design is stark, compact, clear and contrasting. This language carries throughout.

#### Technique

So as mentioned previously, I am serving html templates with GO. This can be pretty simple, we can serve a single basic html page with no modular components. I wanted to try out some fancy tricks using modularized components in this project, cutting down on the size of each template. The way this works within this project is very similar to Vue.js templating. We must declare a template as a component by encapsulating the html with tags.

for example the Navigation bar is coded similarly to any old navbar and saved in templates as navbar.html. The fancy bit is enclosing the code with the tag 

#### Defining a component

``` go
{{ define "navbar" }} Code goes here {{ end }}
```

#### Using a template component

``` go
{{ template "navbar" . }}
```

#### Current Home Page

![ProLUG Home Page](https://trevorsmale.github.io/techblog/images/Sep22site/shot1.png)

#### Current About Page
![ProLUG About Page](https://trevorsmale.github.io/techblog/images/Sep22site/shot2.png)

#### Current Join Page

![ProLUG Join Page](https://trevorsmale.github.io/techblog/images/Sep22site/shot3.png)

#### Current Certify Page

![ProLUG Certify Page](https://trevorsmale.github.io/techblog/images/Sep22site/shot4.png)

#### Current Verify Page

![ProLUG Verify Page](https://trevorsmale.github.io/techblog/images/Sep22site/shot5.png)


### Next Steps... 🥾

1. I plan on discussing this in the Code Cove group in ProLUG
2. I plan on creating a scoreboard as ideated in unit 2 by scott by reading from a CSV and writing to an order list
3. I plan on spiffying up the appearance and useability of the site using tailwind and a bit more js logic

### ProLUG Links ⛓️

Discord: https://discord.com/invite/m6VPPD9usw
Youtube: https://www.youtube.com/@het_tanis8213
Twitch: https://www.twitch.tv/het_tanis
ProLUG Book: https://leanpub.com/theprolugbigbookoflabs
KillerCoda: https://killercoda.com/het-tanis