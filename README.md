# Convert a pdf to an image

This package provides an easy to work with class to convert pdf's to images.

## Fork
This project was forked to enhance the remote file processing.
When a remote file with high number of pages is processed it taked huge time, because Imagick seems to process the whole file, and not just pages required.
We had facing a lot of troubles on own projects with saveImage first page of pdf, with > 300 pages, reaching more than 500 seconds for processing.  
The issue who origined this improvement can be accessed [here](https://github.com/meumobi/sitebuilder/issues/312).

### Results
We have tested with this [file](https://www.rd.com.br/Download.aspx?Arquivo=g1va4C+5cfmJ7zPl9zlMTQ==) (15MB, 556 pages);  
Reducing processing time from 465 sec to 59 sec.



## Requirements

You should have [Imagick](http://php.net/manual/en/imagick.setresolution.php) and [Ghostscript](http://www.ghostscript.com/) installed. See [issues regarding Ghostscript](#issues-regarding-ghostscript).

## Installation

So far the package can be installed via composer, adding on **composer.json**:
``` json
  "repositories": [
        {
            "url": "https://github.com/meumobi/pdf-to-image.git",
            "type": "git"
        }
    ],
    "require": {
        "meumobi/pdf-to-image": "dev-master"
    }
```

And running:
```bash
$ composer install
```

## Usage

Converting a pdf to an image is easy.

```php
$pdf = new Spatie\PdfToImage\Pdf($pathToPdf);
$pdf->saveImage($pathToWhereImageShouldBeStored);
```

If the path you pass to `saveImage` has the extensions `jpg`, `jpeg`, or `png` the image will be saved in that format.
Otherwise the output will be a jpg.

## Other methods

You can get the total number of pages in the pdf:
```php
$pdf->getNumberOfPages(); //returns an int
```

By default the first page of the pdf will be rendered. If you want to render another page you can do so:
```php
$pdf->setPage(2)
    ->saveImage($pathToWhereImageShouldBeStored); //saves the second page
```

You can override the output format:
```php
$pdf->setOutputFormat('png')
    ->saveImage($pathToWhereImageShouldBeStored); //the output wil be a png, no matter what
```

You can set the quality of compression from 0 to 100:
```php
$pdf->setCompressionQuality(100); // sets the compression quality to maximum
```

## Issues regarding Ghostscript

This package uses Ghostscript through Imagick. For this to work Ghostscripts `gs` command should be accessible from the PHP process. For the PHP CLI process (e.g. Laravel's asynchronous jobs, commands, etc...) this is usually already the case. 

However for PHP on FPM (e.g. when running this package "in the browser") you might run into the following problem:

```
Uncaught ImagickException: FailedToExecuteCommand 'gs'
```

This can be fixed by adding the following line at the end of your `php-fpm.conf` file and restarting PHP FPM. If you're unsure where the `php-fpm.conf` file is located you can check `phpinfo()`.

```
env[PATH] = /usr/local/bin:/usr/bin:/bin
```

This will instruct PHP FPM to look for the `gs` binary in the right places.

## Testing

``` bash
$ composer test
```

## Changelog

Please see [CHANGELOG](CHANGELOG.md) for more information what has changed recently.

## Contributing

Please see [CONTRIBUTING](CONTRIBUTING.md) for details.
