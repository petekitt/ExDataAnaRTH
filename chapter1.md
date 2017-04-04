--- 
title_meta  : บทที่ 1 
title       : การจัดการกับข้อมูลด้วย package Dplyr
description : "ในบทเรียนนี้คุณจะได้เรียนรู้เกี่ยวกับการใช้ package Dplyr บนโปรแกรม R ซึ่ง package ดังกล่าวจะช่วยให้การจัดการกับข้อมูลเป็นเรื่องง่ายจนคุณจะต้องตกใจ!"

--- type:NormalExercise lang:r xp:100 skills:1 key:12ff8cde26
## การนำข้อมูลเข้าสู่โปรแกรม R

ก่อนที่เราจะเริ่มลองใช้ package Dplyr ในการจัดการกับฐานข้อมูล เราจะมาเริ่มต้นด้วยการทบทวนเกี่ยวกับการทำความเข้าใจข้อมูลเบื้องต้นโดยการใช้ function R ตามปกติกันก่อน

*** =instructions
ให้คุณนำข้อมูลต่างๆเหล่านี้เข้ามาใน R workspace โดยใช้คำสั่ง `read.delim()` กับไฟล์ `.tsv` ดังต่อไปนี้

- `w_user.tsv`
- `w_restaurant.tsv`
- `w_chain.tsv`
- `w_rating.tsv`
- `w_category.tsv`
- `w_restaurant_category.tsv`
- `w_chain_category.tsv`
- `w_restaurant_checkin_user.tsv`

จากนั้นเก็บข้อมูลจากแต่ละไฟล์ไว้ในตัวแปร(data frame)ที่มีชื่อเดียวกัน เช่น ข้อมูลที่นำเข้าจากไฟล์ `w_user.tsv` ก็ให้เก็บค่าไว้ในตัวแปรชื่อ `w_user`
เราได้พิมพ์ code ตัวอย่างสำหรับการนำเข้าข้อมูล `w_user.tsv` และ `w_restaurant.tsv` ไว้ให้คุณใน editor ทางขวามือแล้ว
สิ่งที่คุณต้องทำคือพิมพ์คำสั่งสำหรับนำข้อมูลอื่นๆที่เหลือเข้ามาใน R workspace!

*** =hint

*** =pre_exercise_code
```{r}
```

*** =sample_code
```{r}
# Import w_user and w_restaurant data to R workspace
# w_user <- read.delim('w_user.tsv')
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')

# Import w_chain, w_rating, w_category, w_restaurant_category, w_chain_category, and w_restaurant_checkin_user to R workspace

```

*** =solution
```{r}
# Import w_user and w_restaurant data to R workspace
# w_user <- read.delim('w_user.tsv')
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')
		
# Import w_chain, w_rating, w_category, w_restaurant_category, w_chain_category, and w_restaurant_checkin_user to R workspace
#w_chain <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_chain.tsv')
#w_rating <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_rating.tsv')
#w_category <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_category.tsv')
#w_restaurant_category <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant_category.tsv')
#w_chain_category <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_chain_category.tsv')
#w_restaurant_checkin_user <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant_checkin_user.tsv')

```

*** =sct
```{r}
#undef_msg <- function(x) {
#	paste("Please make sure that you already created variable `", deparse(substitute(x)), "`", sep="")}
#incor_msg <- function(x) {
#	paste("Please make sure that you imported the correct data into variable `", deparse(substitute(x)), "`. Use function `read.delim()` to import dataset to the variable", sep="")} 
#iteration_list <- c(w_restaurant, w_chain, w_rating, w_category, w_restaurant_category, w_chain_category, w_restaurant_checkin_user)
#for (x in iteration_list) {
#	test_object(x, undefined_msg = undef_msg(x), incorrect_msg = incor_msg(x))}
success_msg("Good job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:999aa2fe6e
## เรามาลองสำรวจตัวอย่างข้อมูลร้านอาหารกัน!

ก่อนที่จะเริ่มทำการวิเคราะห์ข้อมูลร้านอาหาร เราจะลองมาสำรวจลักษณะของข้อมูลร้านอาหารคร่าวๆดูก่อน โดยการใช้ function R ที่เราคุ้นเคย ได้แก่ `head()` `tail()` และ `str()`

*** =instructions
เราได้ import ข้อมูลร้านอาหารไว้ในตัวแปร `w_restaurant` ให้คุณแล้ว
ให้คุณลองใช้ function `head()` และ `tail()` เพื่อตรวจสอบว่าข้อมูล 10 แถวแรกและ 10 แถวสุดท้ายจากตัวแปร `w_restaurant` นั้นเป็นอย่างไร
จากนั้นให้ใช้ function `str()` เพื่อตรวจสอบโครงสร้างของข้อมูลชุดนี้ด้วย

*** =hint

*** =pre_exercise_code
```{r}
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')
```

*** =sample_code
```{r}
# display the first 10 rows from dataset `w_restaurant`
head(w_restaurant, n=10)

# display the last 10 rows from dataset `w_restaurant`


# display structure from the dataset `w_restaurant`

```

*** =solution
```{r}
# display the first 10 rows from dataset `w_restaurant`
head(w_restaurant, n=10)

# display the last 10 rows from dataset `w_restaurant`
tail(w_restaurant, n=10)

# display structure from the dataset `w_restaurant`
str(w_restaurant)
```

*** =sct
```{r}
success_msg("Good job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:628f701efd
## การดึงข้อมูลโดยใช้ Dplyr

*** =instructions
บน editor มีตัวอย่างของการเลือกข้อมูลบางคอลัมน์จากตัวแปร `w_restaurant` แล้วเก็บค่าไว้ในตัวแปร `normal_select` อยู่
ให้คุณลองทำแบบเดียวกันโดยใช้ function `select()` จาก package Dplyr แล้วเก็บผลลัพธ์ไว้ในตัวแปร `dplyr_select`

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')
```

*** =sample_code
```{r}
# without Dplyr
normal_select <- w_restaurant[,c('name', 'price_range', 'parking', 'credit_card_accepted', 'wifi')]

# with Dplyr
deplyr_select <- 
```

*** =solution
```{r}
# without Dplyr
normal_select <- w_restaurant[,c('name', 'price_range', 'parking', 'credit_card_accepted', 'wifi')]

# with Dplyr
deplyr_select <- select(w_restaurant, name, price_range, parking, credit_card_accepted, wifi)
```

*** =sct
```{r}
success_msg("Good job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c3e6460fb1
## การเปลี่ยนชื่อคอลัมน์และการดึงข้อมูลแบบไม่ซ้ำค่าด้วย Dplyr

นอกจากการเลือกข้อมูลบางคอลัมน์ออกมาจาก data frame ตามปกติแล้ว เรายังสามารถเปลี่ยนชื่อคอลัมน์ต่างๆได้ด้วย
และเราสามารถดึงแค่ข้อมูลที่ไม่ซ้ำค่าออกมาได้โดยการใช้ function `distinct()`

*** =instructions
- ให้คุณเลือกข้อมูลคอลัมน์ `name`, `price_range`, `parking` และ `bookable` จาก `w_restaurant` โดยเปลี่ยนชื่อคอลัมน์ `bookable` เป็น `reservable`
- 

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')
```

*** =sample_code
```{r}

```

*** =solution
```{r}

```

*** =sct
```{r}

```

--- type:NormalExercise lang:r xp:100 skills:1 key:0783bbda27
## aaaa


*** =instructions

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}

```

*** =solution
```{r}

```

*** =sct
```{r}

```
