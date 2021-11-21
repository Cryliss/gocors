# go-cors
A tool for scanning domains for CORS misconfigurations written in Go.  
Final project for COMP 424 Software Security  
Professor: Dr. Wonju Lee

By:  
Sabra Bilodeau  
Sally Chung  

## Installation
Clone the repository: `git clone https://github.com/Cryliss/go-cors.git`
Change directories to the repository's directory: `cd go-cors`
Build the application: `make build`

## Usage
`go-cors` can be run using `./go-cors -u https://example.com` for a simple scan on a single URL.  
To run simple scans on multiple URLs, save the URLs to a `.txt` file and run the program like so: `./go-cors -i global_top_100_domains.txt`.  

To add additional configuration to a request, there are two options.  
Either add any of the following command line flags to your input, or update the provided `conf.json` to reflect your desired configuration.   

### CLI flags
-u --url        The URL to scan for CORS misconfiguration
-d --headers    Include headers
-m --method     Include another method other than `GET`
-i --input      A text file with a list of domains to scan for CORS misconfiguration
-t --threads    Number of threads to use for the scan
-o --output     Save the results to a JSON file
-T --timeout    Set requests timeout (default 10s)
-p --proxy      Use a proxy (HTTP)
-h --help       Show the help information & exit
-v --verbose    Enables the UI to display realtime results

`./go-cors -u https://example.com -d "User-Agent: GoogleBot\nCookie: SESSION=Hacked"`

## Misconfigurations Tested
`go-cors` tests the follow CORS misconfigurations:  

- [Origin Reflection]()
- [HTTP Origin]()
- [Null Origin]()
- [Wildcard Origin]()
- [Third Party Origin]()
- [Backtick Bypass]()
- [Pre-Domain Bypass]()
- [Post-Domain Bypass]()
- [Underscore Bypass]()
- [Unescaped Dot Bypass]()

For more information on each, including sample exploits and possible fixes for the vulnerabilities, please click the link provided.
