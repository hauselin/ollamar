
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Ollama R Library

<!-- badges: start -->
<!-- badges: end -->

The Ollama R library provides the easiest way to integrate R with
[Ollama](https://ollama.com/), which lets you run language models
locally on your own machine. For Ollama Python, see
[ollama-python](https://github.com/ollama/ollama-python). You’ll need to
have the [Ollama](https://ollama.com/) app installed on your computer to
use this library.

> Note: You should have at least 8 GB of RAM available to run the 7B
> models, 16 GB to run the 13B models, and 32 GB to run the 33B models.

See [Ollama’s Github page](https://github.com/ollama/ollama) for more
information.

## Installation

1.  You need to have the Ollama app installed on your computer. Download
    it from [Ollama](https://ollama.com/).

2.  After installing the Ollama app, you should open/launch the Ollama
    app to start the local server. Then you can run your language models
    locally, on your own machine/computer.

3.  Install the development version of `ollamar` R library like so:

``` r
devtools::install_github("hauselin/ollamar")
```

If it doesn’t work or you don’t have `devtools` installed, please run
`install.packages("devtools")` in R or RStudio first.

## Usage

``` r
library(ollamar)

# test connection to Ollama server
test_connection()  # returns a httr2 response object
# Ollama local server running
# <httr2_response>
# GET http://localhost:11434/
# Status: 200 OK
# Content-Type: text/plain
# Body: In memory (17 bytes)

# list available models (models you've downloaded)
list_models()  # by default returns a httr2 response object
list_models("df")  # return a data frame of models
# A tibble: 16 × 4
   name                     model                    parameter_size quantization_level
   <chr>                    <chr>                    <chr>          <chr>             
 1 codegemma:instruct       codegemma:instruct       9B             Q4_0              
 2 codellama:python         codellama:python         7B             Q4_0              
 3 gemma:latest             gemma:latest             9B             Q4_0              
 4 llama3:latest            llama3:latest            8B             Q4_0              
```

### Notes

If you don’t have the Ollama app running, you’ll get an error. Make sure
to open the Ollama app before using this library.

``` r
test_connection()
Ollama local server not running or wrong server.
Download and launch Ollama app to run the server. Visit https://ollama.com or https://github.com/ollama/ollama
<error/httr2_failure>
Error in `httr2::req_perform()` at ollamar/R/test_connection.R:18:9:
```

If a function in the library returns an `httr2_response` object, you can
parse the output with `resp_process()`. `ollamar` uses the [`httr2`
library](https://httr2.r-lib.org/index.html) to make HTTP requests to
the Ollama server.

``` r
resp <- list_models()  # by default, returns a httr2 response object 
# <httr2_response>
# GET http://localhost:11434/api/tags
# Status: 200 OK
# Content-Type: application/json
# Body: In memory (5401 bytes)

resp_process(resp, "df")
resp_process(resp, "jsonlist")  # list
resp_process(resp, "raw")  # raw string
resp_process(resp, "resp")  # returns the input httr2 response object
```

### Pull/download model

For the list of models you can pull/download, see [Ollama
library](https://ollama.com/library).

``` r
pull_model("llama3")  # returns a httr2 response object
pull_model("ollama run mistral-openorca")

list_models("df")  # see the models you've downloaded
```

### Chat

``` r
messages <- list(
    list(role = "user", content = "Who is the prime minister of the uk?")
)
chat("llama3", messages)  # returns a httr2 response object
chat("llama3", messages, output = "df")
chat("llama3", messages, output = "raw")
chat("llama3", messages, output = "jsonlist")

messages <- list(
    list(role = "user", content = "Hello!"),
    list(role = "assistant", content = "Hi! How are you?"),
    list(role = "user", content = "Who is the prime minister of the uk?"),
    list(role = "assistant", content = "Rishi Sunak"),
    list(role = "user", content = "List all the previous messages.")
)
chat("llama3", messages)
```

#### Streaming responses

``` r
messages <- list(
    list(role = "user", content = "Hello!"),
    list(role = "assistant", content = "Hi! How are you?"),
    list(role = "user", content = "Who is the prime minister of the uk?"),
    list(role = "assistant", content = "Rishi Sunak"),
    list(role = "user", content = "List all the previous messages.")
)
chat("llama3", messages, stream = TRUE)
```
