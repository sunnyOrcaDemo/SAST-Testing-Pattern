[//]: # (This file is automatically generated. If you wish to make any changes, please use the JSON files and regenerate this file using the tpframework.)

# Buffer

Tags: sast, php, php_v7.4.9

Version: v1.0

## Description

In PHP, [`obj_start()`](https://www.php.net/manual/en/function.ob-start.php) is a function used to start output buffering. This means that any output generated by PHP after the `ob_start()` call will not be sent to the browser or client immediately, but instead will be stored in an internal buffer.
To retrieve the contents of the buffer, you can use the [`ob_get_contents()`](https://www.php.net/manual/en/function.ob-get-contents.php) function. This function returns the contents of the buffer as a string, without clearing the buffer itself.
Once you're done with the output buffering, you can use the [`ob_end_clean()`](https://www.php.net/manual/en/function.ob-end-clean.php) function to discard the contents of the buffer and turn off output buffering. This function removes all data from the output buffer and disables buffering.
Alternatively, you can use the [`ob_get_clean()`](https://www.php.net/manual/en/function.ob-get-clean.php) function, which returns the contents of the output buffer as a string and turns off output buffering in one step. This function is equivalent to calling `ob_get_contents()` followed by `ob_end_clean()`.

## Overview

| Instances                 | has discovery rule   | discovery method   | rule successfull   |
|---------------------------|----------------------|--------------------|--------------------|
| [1 Instance](#1-instance) | yes                  | joern              | yes                |
| [2 Instance](#2-instance) | yes                  | joern              | yes                |

<details markdown="1"open>
<summary>

## 1 Instance
</summary>

The instance shows an example of buffering in PHP. The variable `x` contains everything that should have been written to the output.

### Code

```PHP
<?php
$a = $_GET["p1"]; // source
ob_start();
echo $a;
$x = ob_get_contents();
ob_end_clean();
echo $x; // sink
```

### Instance Properties

| category   | feature_vs_internal_api   | input_sanitizer   | negative_test_case   | source_and_sink   |
|------------|---------------------------|-------------------|----------------------|-------------------|
| S0         | INTERNAL_API              | no                | no                   | no                |

<details markdown="1">
<summary>
<b>More</b></summary>

<details markdown="1">
<summary>

### Compile
</summary>

```bash
$_main:
     ; (lines=13, args=0, vars=2, tmps=7)
     ; (before optimizer)
     ; /.../PHP/75_buffer/1_instance_75_buffer/1_instance_75_buffer.php:1-7
     ; return  [] RANGE[0..0]
0000 T2 = FETCH_R (global) string("_GET")
0001 T3 = FETCH_DIM_R T2 string("p1")
0002 ASSIGN CV0($a) T3
0003 INIT_FCALL 0 80 string("ob_start")
0004 DO_ICALL
0005 ECHO CV0($a)
0006 INIT_FCALL 0 80 string("ob_get_contents")
0007 V6 = DO_ICALL
0008 ASSIGN CV1($x) V6
0009 INIT_FCALL 0 80 string("ob_end_clean")
0010 DO_ICALL
0011 ECHO CV1($x)
0012 RETURN int(1)
```

</details>

<details markdown="1">
<summary>

### Discovery
</summary>

The rule searches for function calls to `ob_start` on opcode level.

```scala
val x75 = (name, "75_buffer_iall", cpg.call.name(".*INIT_FCALL.*").argument.code("ob_start").astParent.location.toJson);
```

| discovery method   | expected accuracy   |
|--------------------|---------------------|
| joern              | Perfect             |

</details>

<details markdown="1"open>
<summary>

### Measurement
</summary>

| Tool        | Comm_1   | Comm_2   | Ground Truth   |
|-------------|----------|----------|----------------|
| 22 May 2023 | yes      | yes      | yes            |

</details>

</details>

</details>

<details markdown="1">
<summary>

## 2 Instance
</summary>

This instance shows, that instead of `ob_get_contents` and `ob_end_clean`, you can use `ob_get_clean` which does the same. It returns the buffered output and ends the buffering.

### Code

```PHP
<?php
$a = $_GET["p1"]; // source
ob_start();
echo $a;
$x = ob_get_clean();
echo $x; // sink
```

### Instance Properties

| category   | feature_vs_internal_api   | input_sanitizer   | negative_test_case   | source_and_sink   |
|------------|---------------------------|-------------------|----------------------|-------------------|
| S0         | FEATURE                   | no                | no                   | no                |

<details markdown="1">
<summary>
<b>More</b></summary>

<details markdown="1">
<summary>

### Compile
</summary>

```bash
$_main:
     ; (lines=11, args=0, vars=2, tmps=6)
     ; (before optimizer)
     ; /.../PHP/75_buffer/2_instance_75_buffer/2_instance_75_buffer.php:1-6
     ; return  [] RANGE[0..0]
0000 T2 = FETCH_R (global) string("_GET")
0001 T3 = FETCH_DIM_R T2 string("p1")
0002 ASSIGN CV0($a) T3
0003 INIT_FCALL 0 80 string("ob_start")
0004 DO_ICALL
0005 ECHO CV0($a)
0006 INIT_FCALL 0 80 string("ob_get_clean")
0007 V6 = DO_ICALL
0008 ASSIGN CV1($x) V6
0009 ECHO CV1($x)
0010 RETURN int(1)
```

</details>

<details markdown="1">
<summary>

### Discovery
</summary>

The rule searches for function calls to `ob_start` on opcode level.

```scala
val x75 = (name, "75_buffer_iall", cpg.call.name(".*INIT_FCALL.*").argument.code("ob_start").astParent.location.toJson);
```

| discovery method   | expected accuracy   |
|--------------------|---------------------|
| joern              | Perfect             |

</details>

<details markdown="1"open>
<summary>

### Measurement
</summary>

| Tool        | Comm_1   | Comm_2   | Ground Truth   |
|-------------|----------|----------|----------------|
| 22 May 2023 | yes      | yes      | yes            |

</details>

</details>

</details>
