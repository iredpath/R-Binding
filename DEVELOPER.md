## IDE
- RStudio
- VSCode with 'R Tools' extension

## Command Line Testing

#### Create A Test Environment

* Launch a Docker environment for testing.
```
docker run -v $(pwd):/R-Binding -it --entrypoint bash r-base
cd /R-Binding/
```

* Install dependencies
```
apt-get update
apt-get install -y libcurl4-openssl-dev libssl-dev libxml2-dev qpdf
Rscript -e 'install.packages("devtools")' \
        -e 'install.packages("knitr")' \
        -e 'install.packages("lintr")' \
        -e 'install.packages("optparse")' \
        -e 'install.packages("rmarkdown")'

```

#### Running Tests

* Run unit tests
```
Rscript -e 'devtools::test()'
```

* Run CRAN submission check
```
Rscript -e 'devtools::check()'
```

* Run lintr on package
```
Rscript -e 'lintr::lint_package()'
```

* Run lintr on example files
```
for file in $(ls examples/*.R); do echo "Rscript -e \"lintr::lint('$file')\"" >> lint_examples.sh; done

chmod a+x lint_examples.sh
./lint_examples.sh

```

* Run an example file
```
(cd examples/; Rscript address_similarity.R --key $API_KEY)
```

* Run all the examples
```
(cd examples/; for file in $(ls *.R); do Rscript ${file} --key $API_KEY; done)
```
