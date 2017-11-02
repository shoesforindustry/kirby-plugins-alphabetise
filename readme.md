# Alphabetise Plugin

## What is it?

The `alphabetise` plugin takes a given [Kirby CMS](http://getkirby.com/) *page* array or *tag* array and returns an alphabetised array that you can then display or further process.



## Installation

### Git submodule

You can download this plugin as a [submodule](https://github.com/blog/2104-working-with-submodules):

```text
git submodule add https://github.com/shoesforindustry/kirby-plugins-alphabetise.git site/plugins/alphabetise
```


### Clone or download

1. [Clone](https://github.com/shoesforindustry/kirby-plugins-alphabetise.git) or [download](https://github.com/shoesforindustry/kirby-plugins-alphabetise/archive/master.zip) this repository.
2. Unzip / Move the folder to `site/plugins` and rename it to `alphabetise`.


### Kirby CLI

Install the plugin:

```text
kirby plugin:install shoesforindustry/kirby-plugins-alphabetise`
```

Update the plugin:

```text
kirby plugin:update shoesforindustry/kirby-plugins-alphabetise
```



## How to use it?

### 1. Alphabetical list of child pages using page title as the key:

* **A**
  * Aa page
  * Ab page
* **B**
  * Ba page
  * Bb page

In your template, call it like this:

```php
<?php
  $sortedPages = $page->children()->visible()->sortBy('title');
  $alphabetise = alphabetise($sortedPages, array('key' => 'title'));
?>
```

The first argument you pass is the sorted **page** array you want to *alphabetise*. The second array's **key** argument determines what to *alphabetise* by. It should be a string like a page 'title'. The values passed to 'sortBy' and 'key' usually are the same.

You then want to loop through the returned results and display them, for example:

```php
<?php foreach($alphabetise as $letter => $items) : ?>
  <h4><?php echo str::upper($letter) ?></h4>
  <ul>
  <?php foreach($items as $item): ?>
    <li>
      <a href="<?php echo $item->url()?>">
        <?php echo $item->title()?>
      </a>
   	</li>
  <?php endforeach ?>
  </ul>
  <hr>
<?php endforeach ?>
```


### 2. Alphabetical list of tags using tag name as the key:

+ **A**
  + Aa tag
  + Ab tag
+ **B**
  + Ba tag
  + Bb tag

**For this to work, the `tagcloud` plugin must be installed!**

In your template, call it like this:

```php
<?php
  $tagPages = $pages->find('pages');
  $alphabetise = alphabetise(tagcloud(($tagPages), array('sort' => 'name','sortdir' => 'asc')), array('key' => 'name'));
?>
```

The first argument you pass is the **tagcloud** array containing the *pages* whose *tags* you want to *alphabetise* (see the [taglcoud plugin documentation](https://github.com/bastianallgeier/kirbycms-extensions/blob/master/plugins/tagcloud/tagcloud.php) for more information). The second array's **key** argument determines what to *alphabetise* by. It should be a string like a tag 'name'.

You then want to loop through the returned results and display them - **be aware** that we're using *$item->name* instead of *item->title* as tags don't have titles - for example:

```php
<?php foreach($alphabetise as $letter => $items) : ?>
  <h4><?php echo str::upper($letter) ?></h4>
  <ul>
  <?php foreach($items as $item): ?>
   	<li>
   	  <a href="<?php echo $item->url()?>">
   	    <?php echo $item->name()?>
      </a>
   	</li>
  <?php endforeach ?>
  </ul>
  <hr>
<?php endforeach ?>
```

You can use any valid array element, so for tags you can also add **$item->results()** for example, which returns the number of items with that tag:

```php
<li>
  <a href="<?php echo $item->url()?>">
    <?php echo $item->name() . ' (' . ($item->results()) . ')' ?>
  </a>
</li>
```


### 3. Set 'orderBy' key:

Version 0.0.9 adds a key to alter how the array appears, by default letters before numbers, e.g.

+ A
+ B
+ 1
+ 2

Or you can set the `orderby` key to `SORT_STRING` so numbers are listed first, e.g.

+ 1
+ 2
+ A
+ B

```php
<?php
  $sortedPages = $page->children()->visible()->sortBy('title');
  $alphabetise = alphabetise($sortedPages, array('key' => 'title', 'orderby'=>SORT_STRING)));
?>
```



## Notes:

The array whose *key* your are trying to sort by should of course only contain letters of the alphabet, otherwise problems may occur.

Also the code (explode) uses a `~` tilde - if you use this in your *key*, especially at the beginning of the string, then you could run into sorting problems. You could of course manually change it if required.

*We are using `ksort`, so other `sort_flags` might be possible, but are untested!*

**The `orderby` key is not a string!**



## Author
Russ Baldwin  
[shoesforindustry.net](shoesforindustry.net)
