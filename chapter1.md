--- 
title_meta  : บทที่ 1 
title       : การจัดการกับข้อมูลด้วย package Dplyr
description : "ในบทเรียนนี้คุณจะได้เรียนรู้เกี่ยวกับการใช้ package Dplyr บนโปรแกรม R ซึ่ง package ดังกล่าวจะช่วยให้การจัดการกับข้อมูลเป็นเรื่องง่ายจนคุณจะต้องตกใจ!"

--- type:NormalExercise lang:r xp:100 skills:1 key:12ff8cde26
## การนำข้อมูลเข้าสู่โปรแกรม R

ก่อนที่เราจะเริ่มลองใช้ package Dplyr ในการจัดการกับฐานข้อมูล เราจะมาเริ่มต้นด้วยการทบทวนเกี่ยวกับการทำความเข้าใจข้อมูลเบื้องต้นโดยการใช้ function R ตามปกติกันก่อน

*** =instructions
ให้คุณนำข้อมูลต่างๆเหล่านี้เข้ามาใน R workspace โดยใช้คำสั่ง read.delim() กับไฟล์ tsv ดังต่อไปนี้

- w_user.tsv
- w_restaurant.tsv
- w_chain.tsv
- w_rating.tsv
- w_category.tsv
- w_restaurant_category.tsv
- w_chain_category.tsv
- w_restaurant_checkin_user.tsv

เก็บข้อมูลจากแต่ละไฟล์ไว้ในตัวแปร(data frame)ที่มีชื่อเดียวกัน เช่น ข้อมูลที่นำเข้าจากไฟล์ w_user.tsv ก็ให้เก็บค่าไว้ในตัวแปรชื่อ w_user
เราได้พิมพ์ code ตัวอย่างสำหรับการนำเข้าข้อมูล w_user และ w_restaurant ไว้ให้คุณใน editor ทางขวามือแล้ว
สิ่งที่คุณต้องทำคือพิมพ์คำสั่งสำหรับนำข้อมูลื่นๆที่เหลือเข้ามาใน R workspace!

*** =hint

*** =pre_exercise_code
```{r}
```

*** =sample_code
```{r}
# Import w_user and w_restaurant data to R workspace
#w_user <- read.delim('w_user.tsv')
#w_restaurant <- read.delim('w_restaurant.tsv', encoding='UTF-8')

# Import w_chain, w_rating, w_category, w_restaurant_category, w_chain_category, and w_restaurant_checkin_user to R workspace

```

*** =solution
```{r}
# Import w_user and w_restaurant data to R workspace
#w_user <- read.delim('w_user.tsv')
#w_restaurant <- read.delim('w_restaurant.tsv', encoding='UTF-8')
		
# Import w_chain, w_rating, w_category, w_restaurant_category, w_chain_category, and w_restaurant_checkin_user to R workspace
#w_chain <- read.delim('w_chain.tsv')
#w_rating <- read.delim('w_rating.tsv')
#w_category <- read.delim('w_category.tsv')
#w_restaurant_category <- read.delim('w_restaurant_category.tsv')
#w_chain_category <- read.delim('w_chain_category.tsv')
#w_restaurant_checkin_user <- read.delim('w_restaurant_checkin_user.tsv')

```

*** =sct
```{r}
undef_msg <- function(x) {return paste("Please make sure that you already create variable `", 
	deparse(substitute(x)), "`", sep="")}
incor_msg <- function(x) {return paste("Please make sure that you import the correct data into variable `", 
	deparse(substitute(x)), "`. Use function `read.delim()` to import dataset to the variable", sep="")} 
iteration_list <- c(w_user, w_restaurant, w_chain, w_rating, w_category, w_restaurant_category, w_chain_category, w_restaurant_checkin_user)
for (x in iteration_list) {
	test_object(x, undefined_msg = undef_msg(x), incorrect_msg = incor_msg(x))}
success_msg("Good job!")
```
