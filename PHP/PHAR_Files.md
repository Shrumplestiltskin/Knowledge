### Create PHAR File

```
<?php
$pharFile = 'app.phar';
$p = new Phar($pharFile);
$p->buildFromDirectory('app/');
$p->setDefaultStub('index.php', '/index.php');
$p->compress(Phar::GZ);
echo "$pharFile successfully created";
```

Given a directory app with a single file "index.php" this will create a phar file named "app.phar" 

```
phar list -f app.phar
\-phar:///home/herman/temp/phar/test/app.phar/index.php
```
