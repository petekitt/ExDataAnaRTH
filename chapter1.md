--- 
title_meta  : บทที่ 1 
title       : การจัดการกับข้อมูลด้วย package Dplyr ระดับเบื้องต้น
description : "ในบทเรียนนี้คุณจะได้เรียนรู้เกี่ยวกับการใช้ package Dplyr ระดับเบื้องต้นบนโปรแกรม R ซึ่ง package ดังกล่าวจะช่วยให้การจัดการกับข้อมูลเป็นเรื่องง่ายจนคุณจะต้องตกใจ!"

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

Dplyr มีตัวช่วยมากมายในการเลือกข้อมูลคอลัมน์ต่างๆออกมาจาก data frame ของคุณ โดยการใช้งานร่วมกับ function `select()` ยกตัวอย่างเช่น:
- คุณสามารถพิมพ์คำสั่ง `select(w_restaurant, starts_with("good"))` เพื่อดึงเฉพาะคอลัมน์ที่มีชื่อขึ้นต้นด้วย "good" ได้
- คุณสามารถพิมพ์คำสั่ง `select(w_restaurant, ends_with("id"))` เพื่อดึงเฉพาะคอลัมน์ที่มีชื่อลงท้ายด้วย "id" ได้
- หรือแม้กระทั่งการใช้คำสั่ง `select(w_restaurant, ends_with("name"))` เพื่อดึงเฉพาะคอลัมน์ที่มีคำว่า "name" อยู่ในชื่อก็สามารถทำได้เช่นเดียวกัน
(คุณสามารถพิมพ์คำสั่งต่างๆลงไปใน Console เพื่อลองทดสอบดูได้เลย)

ซึ่งนอกจากการเลือกคอลัมน์ต่างๆออกมาจาก data frame แล้ว เรายังสามารถเปลี่ยนชื่อคอลัมน์ต่างๆได้ด้วย ยกตัวอย่างเช่น
คำสั่ง `select(w_restaurant, id, name, reservable = bookable)` จะทำการเลือกคอลัมน์ `id`, `name` และ `bookable` ออกมาจาก `w_restaurant` และเปลี่ยนชื่อคอลัมน์ `bookable` เป็น `reservable`

และเราสามารถใช้ function `distinct()` ในการที่เราจะดูว่า data frame ที่เราใส่ลงไปมีค่าที่ไม่ซ้ำกันกี่ค่าบ้าง ยกตัวอย่างเช่น
คำสั่ง `distinct(w_restaurant, opening_date)` จะแสดงผลลัพธ์เป็นวันทั้งหมดที่ร้านอาหารเริ่มเปิดโดยดึงมาเฉพาะค่าที่ไม่ซ้ำกัน (คล้ายกับ function `unique()` ในภาษา R ปกติ)

*** =instructions
- ทำการเลือกข้อมูลจาก `w_user` โดยเลือกมาแค่คอลัมน์ `id`, `gender`, `n_reviews`, `n_ratings`, และ `n_1_ratings` ไปจนถึง `n_5_ratings` เปลี่ยนชื่อคอลัมน์ `n_reviews` เป็น `reviews_count` แล้วเก็บผลลัพธ์ดังกล่าวไว้ในตัวแปร `w_user_new`
- สั่งให้ R แสดงค่า 10 แถวแรกของตัวแปร `w_user_new`
- ใช้ function `distinct()` กับ `w_user_new` เพื่อตรวจสอบว่า dataset ผู้ใช้งานของเรานั้นมีแค่เพศชายและหญิงจริงๆ (`gender` มีค่าเป็น 0 และ 1 ตามลำดับ) แล้วเก็บผลลัพธ์ไว้ในตัวแปร `distinct_gender`
- สั่งให้ R แสดงค่าตัวแปร `distinct_gender`

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')
w_user <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_user.tsv')
```

*** =sample_code
```{r}
# define `w_user_new` here using `select()` function, replace `...` with the arguments you need
w_user_new <- select(...)

# print out the the first 10 rows in `w_user_new`


# now, let's see the distinct gender in our `w_user_new` data frame, store result in `distinct_gender`
distinct_gender <-

# print out distinct values we stored in `distinct_gender`

```

*** =solution
```{r}
# define `w_user_new` here using `select()` function, replace `...` with the arguments you need
w_user_new <- select(w_user, id, gender, reviews_count = n_reviews, contains("ratings"))

# print out the the first 10 rows in `w_user_new`
head(w_user_new, n=10)

# now, let's see the distinct gender in our `w_user_new` data frame, store result in `distinct_gender`
distinct_gender <- distinct(w_user_new, gender)

# print out distinct values we stored in `distinct_gender`
distinct_gender
```

*** =sct
```{r}
success_msg("Good! Now you know how to easily select, change the variable name as you need, and distinguish a redundant values in the dataset. We will move to the next exercise!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:0783bbda27
## filter() และ mutate()!

การคัดกรองข้อมูล และการคำนวณหรือเปลี่ยนแปลงรูปแบบข้อมูลให้เหมาะสม เป็นสิ่งที่หลีกเลี่ยงไม่ได้ในการวิเคราะห์ข้อมูล
เราสามารถใช้ function `filter()` ในการเลือกคัดกรองเฉพาะกลุ่มข้อมูลที่เราสนใจได้
ส่วน function `mutate()` นั้นจะช่วยให้เราสามารถเปลี่ยนแปลงรูปแบบของข้อมูล หรือแม้กระทั่งสร้างตัวแปรใหม่ที่เกิดจากการคำนวณด้วยตัวแปรอื่นๆในข้อมูลของเราได้ โดย function `mutate()` จะทำการเพิ่มตัวแปรใหม่เข้าไปใน dataset เดิมของเรา

- and maybe we put some demonstration on how to simply use filter() and mutate() on `w_restaurant` here filtering just only the restaurant that have wifi then mutate the parking column to say something like have or don't have parking lots

*** =instructions
ต่อจากแบบฝึกหัดที่แล้ว สิ่งที่คุณต้องทำกับตัวแปร `w_user_new` คือ
- ทำการกรองข้อมูลผู้ใช้ โดยใช้ function `filter()` เลือกเฉพาะผู้ใช้ที่เคยเขียนรีวิวร้านอาหารให้กับเว็บไซต์ wongnai (หรือก็คือ reviews_count > 0)
- เมื่อทำการคัดกรองข้อมูลผู้ใช้เรียบร้อยแล้ว ให้คุณใช้ function `mutate()` ในการเพิ่มคอลัมน์ `avg_rating` ที่เก็บค่าเฉลี่ยของ rating ที่ user แต่ละคนให้กับร้านอาหาร
- เก็บผลลัพธ์สุดท้ายไว้ในตัวแปร `mutate_w_user`
- สั่งให้ R แสดงค่า 10 แถวแรกจาก `mutate_w_user`
เราได้ทำการเตรียมตัวแปร `w_user_new` ไว้ให้คุณแล้ว คุณสามารถลองสำรวจตัวแปร `w_user_new` นี้ได้โดยการพิมพ์ `str(w_user_new)` ลงไปใน Console

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_user <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_user.tsv')
w_user_new <- select(w_user, id, gender, reviews_count = n_reviews, contains("ratings"))
```

*** =sample_code
```{r}
# or even columns that contain the same word by using `contains()` 
filtered_user <- w_user_new[w_user_new$reviews_count > 0, ]
filtered_user$avg_rating <- (filtered_user$n_1_ratings + 2*filtered_user$n_2_ratings + 3*filtered_user$n_3_ratings + 4*filtered_user$n_4_ratings + 5*filtered_user$n_5_ratings) / (filtered_user$n_1_ratings + filtered_user$n_2_ratings + filtered_user$n_3_ratings + filtered_user$n_4_ratings + filtered_user$n_5_ratings)

# do the same thing in `Dplyr` way



# print out sample results

```

*** =solution
```{r}
# doing the job based on original R approach
filtered_user <- w_user_new[w_user_new$reviews_count > 0, ]
filtered_user$avg_rating <- (filtered_user$n_1_ratings + 2*filtered_user$n_2_ratings + 3*filtered_user$n_3_ratings + 4*filtered_user$n_4_ratings + 5*filtered_user$n_5_ratings) / (filtered_user$n_1_ratings + filtered_user$n_2_ratings + filtered_user$n_3_ratings + filtered_user$n_4_ratings + filtered_user$n_5_ratings)

# do the same thing in `Dplyr` way
mutate_w_user <- mutate(filter(w_user_new, reviews_count > 0), avg_rating = (n_1_ratings + 2*n_2_ratings + 3*n_3_ratings +
	4*n_4_ratings + 5*n_5_ratings) / (n_1_ratings + n_2_ratings + n_3_ratings + n_4_ratings + n_5_ratings))

# print out sample results
head(mutate_w_user, n=10)
```

*** =sct
```{r}
success_msg("Good job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:1c38435d99
## bbb


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
