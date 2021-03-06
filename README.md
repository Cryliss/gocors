# gocors
[![Go Reference](https://pkg.go.dev/badge/github.com/Cryliss/gocors.svg)](https://pkg.go.dev/github.com/Cryliss/gocors)  [![GoReportCard example](https://goreportcard.com/badge/github.com/Cryliss/gocors)](https://goreportcard.com/report/github.com/Cryliss/gocors)


A tool for scanning domains for CORS misconfigurations written in Go.  
Final project for COMP 424 Software Security  
Professor: Dr. Wonju Lee

**By:**  
Sabra Bilodeau  
Sally Chung

## Misconfigurations Tested
`gocors` tests the follow CORS misconfigurations:  

- [Backtick Bypass](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/BACKTICK_BYPASS.md)
- [HTTP Origin](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/HTTP_ORIGIN.md)
- [Origin Reflection](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/ORIGIN_REFLECTION.md)
- [Null Origin](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/NULL_ORIGIN.md)
- [Post-Domain Bypass](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/POSTDOMAIN_BYPASS.md)
- [Pre-Domain Bypass](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/PREDOMAIN_BYPASS.md)
- [Special Characters Bypass](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/SPECIAL_CHARACTERS_BYPASS.md)
- [Third Party Origin](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/THIRD_PARTY_ORIGINS.md)
- [Underscore Bypass](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/UNDERSCORE_BYPASS.md)
- [Unescaped Dot Bypass](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/UNESCAPED_DOT_BYPASS.md)
- [Wildcard Origin](https://github.com/Cryliss/gocors/blob/main/docs/misconfigurations/WILDCARD_ORIGIN.md)

For more information on each, including sample exploits and possible fixes for the vulnerabilities, please click the link provided.

## Installation
Clone the repository:  
`git clone https://github.com/Cryliss/gocors.git`  

Change directories to the repository's directory:  
`cd gocors`  

Build the application:  
`make build`  

## Usage
### Simple Scans
To run a scan on a signle URL, use `./gocors -url https://example.com`.  

To run scans on multiple URLs, save the URLs to a `.txt` file and run the program like so:  

`./gocors -input global_top_100_domains.txt`  

### Configurable Scans
To add additional configuration to a request, there are two options.  
1. Add any of the following command line flags to your input  
2. Update the provided `conf.json` to reflect your desired configuration.   

### CLI flags
| Flag | Description | Default |
| :--: | :---------- | :-----: |
| -url     | The URL to scan for CORS misconfiguration | "" |
| -headers | Include headers | "" |
| -method  |  Include another method other than `GET` | "GET" |
| -input   |  A text file with a list of domains or a json configuration file | "" |
| -threads |  Number of threads to use for the scan | 10 |
| -output  |  Directory to save the results to a JSON file. | "" |
| -timeout |  Set requests timeout | "10s" |
| -proxy   |  Use a proxy (HTTP) | "" |
| -h       |  Show the help information & exit | N/A |
| -verbose |  Enables the UI to display realtime results | false |

## Example Usage of the CLI flags  
- **URL**:     `./gocors -url https://example.com`
- **Headers**: `./gocors -url https://example.com -headers "User-Agent: GoogleBot\nCookie: SESSION=Hacked"`
- **Method**:  `./gocors -url https://example.com -method POST`
- **Input**:   `./gocors -input global_top_100_domains.txt`
- **Threads**: `./gocors -url https://example.com -threads 20`
- **Output**:  `./gocors -url https://example.com -output "/path/to/your/results/directory/"`
- **Timeout**: `./gocors -url https://example.com -timeout 20s`
- **Proxy**:   `./gocors -url https://example.com -proxy http://127.0.0.1:4545`
- **Verbose**: `./gocors -url https://example.com -verbose true`


# Using `gocors` in your own application

Run `go get github.com/Cryliss/gocors` in your terminal.

```go
package main

import (
    "github.com/Cryliss/gocors"
    "github.com/Cryliss/gocors/scanner"
)

func main() {
    // Set our scanner configuration variables
    output := "/path/to/your/output/directory"
    timeout := "10s"
    threads := 10

    // Create a new scanner.
    corsScanner := gocors.InitGoCors(output, timeout, threads)

    /*
    In order to start running tests with gocors, we need to create them first.

    Creating tests requires an array of domain names, a scanner.Headers variable
    which is a map[string]string of header name-value pairs, a request method and
    a proxy URL. If you want to set custom headers, do:
    headers["cookie"] = "SESSION=Hacked"

    After creating our headers variable and domain names, then we can call the create
    tests function, which will set scanner.Conf.Tests value at the end.
    */
    var headers scanner.Headers
    domains := []string{"https://www.instagram.com/"}
    corsScanner.CreateTests(domains, headers, "GET", "")

    // Now that we have our tests set, we can go ahead and start the scanner.
    // Once the scan finishes, it will automatically save your results to the output
    // directory, if one is provided.
    corsScanner.Start()
}
```
