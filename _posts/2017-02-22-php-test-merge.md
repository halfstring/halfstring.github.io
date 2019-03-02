---
title: php -> array_merge
date: 2017-02-22
updated: 2017-02-22
tags: php merege
---

php -> array_merge

```
<?php
$a = [
 'a' => 'A',
 'b' => 'B',
 'c' => 'C'
];

$b = [
 'b'=> 'BB',
 'e'=> 'E'
];

$c = $a + $b;
var_dump($c);

$c = array_merge($a, $b);
var_dump($c);
```

```
result:
array(4) {
  ["a"]=>
  string(1) "A"
  ["b"]=>
  string(1) "B"
  ["c"]=>
  string(1) "C"
  ["e"]=>
  string(1) "E"
}
array(4) {
  ["a"]=>
  string(1) "A"
  ["b"]=>
  string(2) "BB"
  ["c"]=>
  string(1) "C"
  ["e"]=>
  string(1) "E"
}
```
