--- 
title_meta  : บทที่ 1 
title       : Introduction to Dplyr
description : "ในบทเรียนนี้คุณจะได้เรียนรู้เกี่ยวกับการใช้ package Dplyr ระดับเบื้องต้นบนโปรแกรม R ซึ่ง package ดังกล่าวจะช่วยให้การจัดการกับข้อมูลเป็นเรื่องง่ายจนคุณจะต้องตกใจ!"


--- type:NormalExercise lang:r xp:100 skills:1 key:12ff8cde26
## การนำข้อมูลเข้าสู่โปรแกรม R

ก่อนที่เราจะเริ่มลองใช้ package Dplyr ในการจัดการกับฐานข้อมูล เราจะมาเริ่มต้นด้วยการทบทวนเกี่ยวกับการทำความเข้าใจข้อมูลเบื้องต้นโดยการใช้ function R ตามปกติกันก่อน

*** =instructions
ให้คุณนำข้อมูลต่างๆเหล่านี้เข้ามาใน R workspace โดยใช้คำสั่ง `read.delim()` กับไฟล์ `.tsv` ดังต่อไปนี้

* `user.tsv`
* `restaurant.tsv`
* `rating.tsv`
* `category.tsv`

จากนั้นเก็บข้อมูลจากแต่ละไฟล์ไว้ในตัวแปร(data frame)ที่มีชื่อเดียวกัน เช่น ข้อมูลที่นำเข้าจากไฟล์ `user.tsv` ก็ให้เก็บค่าไว้ในตัวแปรชื่อ `user`
เราได้พิมพ์ code ตัวอย่างสำหรับการนำเข้าข้อมูล `user.tsv` และ `restaurant.tsv` ไว้ให้คุณใน editor ทางขวามือแล้ว
สิ่งที่คุณต้องทำคือพิมพ์คำสั่งสำหรับนำข้อมูลอื่นๆที่เหลือเข้ามาใน R workspace!

*** =hint

*** =pre_exercise_code
```{r}
```

*** =sample_code
```{r}
# Import `user` and `restaurant` data to R workspace
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")

# Import `rating`, `category` to R workspace

```

*** =solution
```{r}
# Import `user` and `restaurant` data to R workspace
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
		
# Import `rating`, `category` to R workspace
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
category <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/category.tsv")

```

*** =sct
```{r}
undef_msg <- function(x) {
	paste("Please make sure that you already created variable `", x, "`", sep="")
}
incor_msg <- function(x) {
	paste("Please make sure that you imported the correct data into variable `", x, "`. Use function `read.delim()` to import dataset to the variable", sep="")
} 
for (item in c("user", "restaurant", "rating", "category")) {
	test_object(item, undefined_msg = undef_msg(item), incorrect_msg = incor_msg(item))
}
success_msg("Good job!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:999aa2fe6e
## เรามาลองสำรวจตัวอย่างข้อมูลร้านอาหารกัน!

ก่อนที่จะเริ่มทำการวิเคราะห์ข้อมูลร้านอาหาร เราจะลองมาสำรวจลักษณะของข้อมูลร้านอาหารคร่าวๆดูก่อน โดยการใช้ function R ที่เราคุ้นเคย ได้แก่ `head()` `tail()` และ `str()`

*** =instructions
เราได้ import ข้อมูลร้านอาหารไว้ในตัวแปร `restaurant` ให้คุณแล้ว
ให้คุณลองใช้ function `head()` และ `tail()` เพื่อตรวจสอบว่าข้อมูล 10 แถวแรกและ 10 แถวสุดท้ายจากตัวแปร `restaurant` นั้นเป็นอย่างไร
จากนั้นให้ใช้ function `str()` เพื่อตรวจสอบโครงสร้างของข้อมูลชุดนี้ด้วย

*** =hint

*** =pre_exercise_code
```{r}
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# display the first 10 rows from dataset `restaurant`
head(restaurant, n = 10)

# display the last 10 rows from dataset `restaurant`


# display structure from the dataset `restaurant`

```

*** =solution
```{r}
# display the first 10 rows from dataset `restaurant`
head(restaurant, n = 10)

# display the last 10 rows from dataset `restaurant`
tail(restaurant, n = 10)

# display structure from the dataset `restaurant`
str(restaurant)
```

*** =sct
```{r}
test_output_contains("head(restaurant, n = 10)", incorrect_msg = "อย่าลืม Please make sure you already correctly call the `head()` function with argument `n = 10`")
test_output_contains("tail(restaurant, n = 10)", incorrect_msg = "Please make sure you already correctly call the `tail()` function with argument `n = 10`")
test_output_contains("str(restaurant)", incorrect_msg = "Please make sure you already correctly call the `str()` function with `restaurant` as an argument")
success_msg("Good job!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:628f701efd
## การดึงข้อมูลโดยใช้ Dplyr

คุณสามารถเลือกข้อมูลจาก data frame ต่างๆได้โดยใช้คำสั่ง `select()` ยกตัวอย่างเช่น:

- `select(restaurant, name, price_range)` จะทำการเลือกแค่คอลัมน์ `name` และ `price_range` ออกมาจาก data frame `restaurant`
- `select(restaurant, 1, 2, 3)` จะทำการเลือกแค่คอลัมน์ที่ 1, 2 และ 3 ออกมาจาก data frame `restaurant`

argument ที่ 1 ของ `select()` จะเป็นตัวแทนของ data frame ที่คุณต้องการจะดึงข้อมูลออกมา

*** =instructions
บน editor มีตัวอย่างของการเลือกข้อมูลบางคอลัมน์จากตัวแปร `restaurant` แล้วเก็บค่าไว้ในตัวแปร `normal_selection` อยู่
ให้คุณลองทำแบบเดียวกันโดยใช้ function `select()` จาก package Dplyr แล้วเก็บผลลัพธ์ไว้ในตัวแปร `dplyr_selection`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# without Dplyr
normal_selection <- restaurant[, c("name", "price_range", "parking", "credit_card_accepted", "wifi")]

# with Dplyr
dplyr_selection <- 
```

*** =solution
```{r}
# without Dplyr
normal_selection <- restaurant[, c("name", "price_range", "parking", "credit_card_accepted", "wifi")]

# with Dplyr
dplyr_selection <- select(restaurant, name, price_range, parking, credit_card_accepted, wifi)
```

*** =sct
```{r}
success_msg("Good job!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:9c0aecf43f
## การเปลี่ยนชื่อคอลัมน์

นอกจากการเลือกข้อมูลโดยพิมพ์ชื่อคอลัมน์ของคอลัมน์นั้นๆลงไปใน fucntion `select()` คุณยังสามารถเปลี่ยนชื่อคอลัมน์ที่คุณดึงออกมาได้อีกด้วย

คำสั่ง `select(restaurant, id, name, parking_lot = parking)` จะทำการเลือกคอลัมน์ `id`, `name`, และ `parking` ออกมาจาก `restaurant` แล้วเปลี่ยนชื่อคอลัมน์ `parking` เป็น `parking_lot`

*** =instructions
- ให้คุณเลือกคอลัมน์ `id`, `name` และ `bookable` จาก data frame `restaurant` แล้วเปลี่ยนชื่อคอลัมน์เหล่านี้เป็น `restaurant_id`, `restaurant_name` และ `reservable` ตามลำดับ แล้วเก็บค่าไว้ในตัวแปรชื่อ `answer`
- แสดงค่าข้อมูล 10 แถวแรกจากตัวแปร `answer`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
answer <- select(...)
```

*** =solution
```{r}
answer <- select(restaurant, restaurant_id = id, restaurant_name = name, reservable = bookable)

head(answer, n = 10)
```

*** =sct
```{r}
success_msg("Great!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:c3e6460fb1
## การเลือกคอลัมน์โดยดูจากชื่อขึ้นต้น

Dplyr มีตัวช่วยมากมายในการเลือกข้อมูลคอลัมน์ต่างๆออกมาจาก data frame ของคุณ โดยการใช้งานร่วมกับ function `select()` เช่น `start_with()`, `ends_with()` และ `contains()` ซึ่งเราจะเรียก function เหล่านี้ว่า `helper function`

คำสั่ง `starts_with()` จะช่วยให้คุณเลือกคอลัมน์โดยดูจากชื่อขึ้นต้นของคอลัมน์นั้นๆได้ ยกตัวอย่างเช่น

`select(restaurant, starts_with("good"))` จะดึงทุกๆคอลัมน์ใน `restaurant` ที่มีชื่อคอลัมน์ขึ้นต้นด้วย "good" ออกมา (คุณสามารถพิมพ์คำสั่งต่างๆลงไปใน Console เพื่อลองทดสอบดูได้เลย)

*** =instructions
- ทำการเลือกข้อมูลจาก `user` โดยเลือกมาแค่คอลัมน์ `id`, `gender`, `n_reviews`, `n_ratings`, และ `n_1_ratings` ไปจนถึง `n_5_ratings` แล้วเก็บผลลัพธ์ดังกล่าวไว้ในตัวแปร `user_new` (`starts_with()` สามารถช่วยคุณให้คุณไม่ต้องพิมพ์คำสั่งยาวๆได้ในจุดนี้)
- สั่งให้ R แสดงค่า 10 แถวแรกของตัวแปร `user_new`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# define `user_new` here using `select()` function, replace `...` with the arguments you need
user_new <- select(...)

# print out the the first 10 rows in `user_new`


# now, let's see the distinct gender in our `user_new` data frame, store result in `distinct_gender`
distinct_gender <-

# print out distinct values we stored in `distinct_gender`

```

*** =solution
```{r}
# define `user_new` here using `select()` function, replace `...` with the arguments you need
user_new <- select(user, id, gender, reviews_count = n_reviews, contains("ratings"))

# print out the the first 10 rows in `user_new`
head(user_new, n = 10)
```

*** =sct
```{r}
success_msg("Good! Now you can see that we actually have 3 genders in our user data - 1 for male, 2 for female, and 0 for the unspecified. Now, we will move to the next exercise!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:21152f4415
## การเลือกคอลัมน์โดยดูจากชื่อลงท้าย

ตรงกันข้ามกับ function `starts_with()` ที่เราได้เรียนรู้ไปในแบบฝึกหัดก่อนหน้านี้ function `ends_with()` จะทำการเลือกเฉพาะคอลัมน์ที่ลงท้ายด้วยคำที่คุณกำหนด

*** =instructions
- ทำการเลือกข้อมูลจาก `user` โดยเลือกมาแค่คอลัมน์ `id`, `gender` และทุกคอลัมน์ที่ลงท้ายด้วยคำว่า `ratings`
- เก็บผลลัพธ์ไว้ในตัวแปร `user_new` และแสดงค่าข้อมูล 10 แถวแรกออกมา

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# select id, gender, and all the columns that end with `ratings` in the column names and store the results in `user_new`
user_new <- select(user, ...)

#print out the first 10 rows from `user_new`

```

*** =solution
```{r}
# select id, gender, and all the columns that end with `ratings` in the column names and store the results in `user_new`
user_new <- select(user, id, gender, ends_with("ratings"))

#print out the first 10 rows from `user_new`
head(user_new, n = 10)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:139d1b8d08
## การเลือกคอลัมน์โดยดูจากคำที่อยู่ในชื่อ

อีกหนึ่งตัวอย่างคือการเลือกเฉพาะคอลัมน์ที่มีคำที่คุณกำหรดอยู่ในชื่อคอลัมน์ออกมา ซึ่งคุณสามารถทำได้โดยการใช้คำสั่ง `contains()`

ที่จริงแล้วยังมี function อื่นๆที่ช่วยในการเลือกคอลัมน์อีกมากมาย ซึ่งคุณสามารถศึกษาเพิ่มเติมได้จากการพิมพ์ `?dplyr::select_helpers` ลงไปใน Console

*** =instructions
- ลองใช้ function `select()` กับตัวแปร `user` อีกครั้ง แต่คราวนี้ให้เลือกทุกลัมน์ที่ประกอบด้วย `_` ในชื่อออกมา แล้วเก็บผลลัพธ์ไว้ในตัวแปร `user_new`
- แสดงค่าข้อมูล 20 แถวแรกจาก `user_new`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# your code here


```

*** =solution
```{r}
# your code here
user_new <- select(user, contains("_"))
head(user_new, n = 20)
```

*** =sct
```{r}
success_msg("Cool!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:d8f571fd73
## การแสดงค่าทั้งหมดที่เป็นไปได้ของแต่ละคอลัมน์โดยใช้ distinct()

เราสามารถใช้ function `distinct()` ในการที่เราจะดูว่า data frame ที่เราใส่ลงไปมีค่าที่ไม่ซ้ำกันกี่ค่าบ้าง
คำสั่ง `distinct(restaurant, opening_date)` จะแสดงผลลัพธ์เป็นวันทั้งหมดที่ร้านอาหารเริ่มเปิดโดยดึงมาเฉพาะค่าที่ไม่ซ้ำกัน (คล้ายกับ function `unique()` ในภาษา R ปกติ)

*** =instructions
- ใช้ function `distinct()` กับ `user` เพื่อตรวจสอบว่า dataset ผู้ใช้งานของเรานั้นมีแค่เพศชายและหญิงจริงๆ (`gender` มีค่าเป็น 0 และ 1 ตามลำดับ) แล้วเก็บผลลัพธ์ไว้ในตัวแปร `distinct_gender`
- สั่งให้ R แสดงค่าตัวแปร `distinct_gender`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# let's see the distinct gender in our `user_new` data frame, store result in `distinct_gender`
distinct_gender <- distinct(user_new, ...)

# print out distinct values we stored in `distinct_gender`

```

*** =solution
```{r}
# let's see the distinct gender in our `user_new` data frame, store result in `distinct_gender`
distinct_gender <- distinct(user_new, gender)

# print out distinct values we stored in `distinct_gender`
distinct_gender
```

*** =sct
```{r}
success_msg("Great!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:0783bbda27
## คัดกรองข้อมูลตามเงื่อนไขด้วย filter()!

การคัดกรองข้อมูล และการคำนวณหรือเปลี่ยนแปลงรูปแบบข้อมูลให้เหมาะสม เป็นสิ่งที่หลีกเลี่ยงไม่ได้ในการวิเคราะห์ข้อมูล
เราสามารถใช้ function `filter()` ในการเลือกคัดกรองเฉพาะกลุ่มข้อมูลที่เราสนใจได้

*** =instructions
เติมคำลงใน `...` เพื่อทำตามคำสั่งต่อไปนี้:
- ให้คุณสร้างตัวแปร `user_new` โดยการเลือกคอลัมน์ `id`, `gender`, `n_reviews`, และทุกคอลัมน์ที่ลงท้ายด้วย `ratings` เปลี่ยนชื่อคอลัมน์ `n_reviews` เป็น `reviews_count`
- ทำการกรองข้อมูลผู้ใช้ โดยใช้ function `filter()` เลือกเฉพาะผู้ใช้ที่เคยเขียนรีวิวร้านอาหารให้กับเว็บไซต์ wongnai (หรือก็คือ `reviews_count` > 0)
- เก็บผลลัพธ์จากข้อก่อนหน้านี้ไว้ในตัวแปร `filtered_user_new`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# create `user_new` here and do not forget to change `n_reviews` column name to `reviews_count`
user_new <- select(user, id, gender, ... = n_reviews, ends_with(...))

# do the same thing in `Dplyr` way and store result in `filtered_user_new`

```

*** =solution
```{r}
# create `user_new` here and do not forget to change `n_reviews` column name to `reviews_count`
user_new <- select(user, id, gender, reviews_count = n_reviews, ends_with("ratings"))

# do the same thing in `Dplyr` way and store result in `filtered_user_new`
filtered_user_new <- filter(user_new, reviews_count > 0)
```

*** =sct
```{r}
success_msg("Good job!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:03371e8ac0
## การสร้างคอลัมน์ใหม่จากการคำนวณด้วย mutate()!

function `mutate()` นั้นจะช่วยให้เราสามารถเปลี่ยนแปลงรูปแบบของข้อมูล หรือแม้กระทั่งสร้างตัวแปรใหม่ที่เกิดจากการคำนวณด้วยตัวแปรอื่นๆในข้อมูลของเราได้ โดย function `mutate()` จะทำการเพิ่มตัวแปรใหม่เข้าไปใน dataset เดิมของเรา

*** =instructions
เราได้เตรียม `filtered_user_new` จากแบบฝึกหัดที่แล้วไว้ให้คุณแล้ว กรุณาปฏิบัติตามคำสั่งดังต่อไปนี้:
- ให้คุณใช้ function `mutate()` กับ `filtered_user_new` ในการเพิ่มคอลัมน์ `avg_rating` ที่เก็บค่าเฉลี่ยของ rating ที่ user แต่ละคนให้กับร้านอาหาร
- เก็บผลลัพธ์สุดท้ายไว้ในตัวแปร `user_with_rating`
- สั่งให้ R แสดงค่า 10 แถวแรกจาก `user_with_rating`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
filtered_user_new <- filter(select(user_new, id, gender, reviews_count = n_reviews, ends_with("ratings")), reviews_count > 0)
```

*** =sample_code
```{r}
# sample code is for finding average ratings from 1 to 3. Change it to finding average from 1 to 5 so you get the correct answer
user_with_rating <- mutate(filtered_user_new,
	avg_ratings = (n_1_ratings + 2 * n_2_ratings + 3 * n_3_ratings) /
	(n_1_ratings + n_2_ratings + n_3_ratings)
)

# print out sample results

```

*** =solution
```{r}
# sample code is for finding average ratings from 1 to 3. Change it to finding average from 1 to 5 so you get the correct answer
user_with_rating <- mutate(filtered_user_new,
	avg_ratings = (n_1_ratings + 2 * n_2_ratings + 3 * n_3_ratings + 4 * n_4_ratings + 5 * n_5_ratings) /
	(n_1_ratings + n_2_ratings + n_3_ratings + n_4_ratings + n_5_ratings)
)

# print out sample results
head(user_with_rating, n = 10)
```

*** =sct
```{r}
success_msg("Good Job!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:1c38435d99
## การสรุปข้อมูลด้วย summarise()!

ในแบบฝึกหัดที่แล้ว เราได้ทำการคัดกรองข้อมูลเฉพาะผู้ใช้ที่มีการเขียนรีวิวลงบนเว็บไซต์ และคำนวณคะแนนเฉลี่ยที่ผู้ใช้แต่ละคนจะให้กับร้านอาหารต่างๆ แล้วเก็บไว้ในตัวแปร `user_with_rating`

คราวนี้เราจะมาลองวิเคราะห์ภาพรวมของข้อมูลกันโดยใช้ function `summarise()`

function `summarise()` จะช่วยให้เราสามารถสรุปข้อมูลได้ตามที่เราต้องการ โดยขึ้นอยู่กับว่าเราใส่อะไรลงไปเป็น argument ของ function `summarise()` ยกตัวอย่างเช่น:

- คำสั่ง `summarise(user, n())` จะทำการสรุปข้อมูลให้เราว่าข้อมูลใน data frame `user` มีทั้งหมดกี่แถว (หรือก็คือมีผู้ใช้จำนวนกี่คน)
- คำสั่ง `summarise(user, mean(n_reviews), sum(n_3_ratings), median(n_followers))` จะแสดงผลลัพธ์เป็นจำนวน review เฉลี่ยต่อผู้ใช้ 1 คน, จำนวน rating ระดับ 3 ทั้งหมดที่ผู้ใช้ใน `user` ได้ให้กับร้านอาหาร และค่ามัธยฐานของจำนวนผู้ติดตามของผู้ใช้แต่ละคนใน `user`

คุณสามารถลองเล่นกับ function `summarise()` นี้ได้โดยการนำ code เหล่านี้ไปพิมพ์ใน Console และลองดูว่าผลเป็นอย่างไร

*** =instructions
ให้คุณทำการวิเคราะห์ข้อมูลผู้ใช้ต่อโดยใช้ข้อมูลจากตัวแปร `user_with_rating` ที่ถูกเตรียมไว้ให้ใน workspace แล้ว
- ทำการสรุปข้อมูลโดยใช้ function `summarise()` หาว่า `user_with_rating` มีจำนวน `reviews_count` โดยเฉลี่ย, จำนวน `n_photos` โดยเฉลี่ย, จำนวน `n_followers` โดยเฉลี่ย, ค่าเฉลี่ยและส่วนเบี่ยงเบนมาตรฐานของ `avg_rating` เป็นเท่าไรบ้าง
- คุณควรใช้ function `mean()` และ `sd()` ร่วมกับ `summarise()` ในการหาค่าเฉลี่ยและส่วนเบี่ยงเบนมาตรฐาน
- ไม่ต้องตั้งชื่อให้กับแต่ละคอลัมน์ และให้ R แสดงผลลัพธ์ออกมาได้เลยโดยไม่ต้องเก็บผลลัพธ์ไว้ในตัวแปรใดๆ

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")

user_new <- select(user, id, gender, reviews_count = n_reviews, contains("ratings"), n_photos, n_followers)

user_with_rating <- mutate(
	filter(user_new, reviews_count > 0), 
	avg_rating = (n_1_ratings + 2 * n_2_ratings + 3 * n_3_ratings + 4 * n_4_ratings + 5 * n_5_ratings) / 
	(n_1_ratings + n_2_ratings + n_3_ratings + n_4_ratings + n_5_ratings)
)
```

*** =sample_code
```{r}
# This will return summary information about average amount of total level 1 rating through level 3 rating per 1 user
summarise(user, mean(n_1_ratings + n_2_ratings + n_3_ratings))

# Your code here
summarise(
	user_with_rating, 
	mean(...), mean(...), mean(...), 
	mean(...), sd(...)
)
```

*** =solution
```{r}
# This will return summary information about average amount of total level 1 rating through level 3 rating per 1 user
summarise(user, mean(n_1_ratings + n_2_ratings + n_3_ratings))

# Your code here
summarise(
	user_with_rating, 
	mean(reviews_count), mean(n_photos), mean(n_followers), 
	mean(avg_rating), sd(avg_rating)
)
```

*** =sct
```{r}
success_msg("Well done!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:ee82ffad79
## สรุปข้อมูลให้ดีขึ้นและละเอียดขึ้นด้วย group_by()!

ในการวิเคราะห์ข้อมูลในการทำงานจริง เราอาจจะอยากทำการวิเคราะห์ให้ลึกลงไปโดยแบ่งข้อมูลที่เรามีตามกลุ่มต่างๆด้วย ซึ่ง function `group_by()` จะสามารถช่วยเราได้ในจุดนี้

หากคุณลองพิมพ์คำสั่ง `summarise(group_by(restaurant, price_range), n(), sum(verified_info), verified_rate = sum(verified_info) / n())` ลงไปใน Console สิ่งที่คุณจะเห็นจากผลลัพธ์คือการสรุปข้อมูลของจำนวนร้านอาหารทั้งหมด, จำนวนร้านที่ได้รับการยืนยันข้อมูลแล้ว และอัตราการยืนยันข้อมูล โดยแบ่งกลุ่มตาม `price_range`

ซึ่งจากผลลัพธ์ดังกล่าว คุณจะเห็นได้ว่าร้านอาหารที่มี `price_range` จัดอยู่ในระดับสูงกว่า(ราคาอาหารแพงกว่า) มีแนวโน้มที่จะได้รับการยืนยันข้อมูลมากกว่าอย่างชัดเจน

*** =instructions
ต่อจากแบบฝึกหัดที่แล้วกับตัวแปร `user_with_rating`
- คล้ายกับแบบฝึกหัดที่แล้ว ให้คุณทำการสรุปข้อมูลโดยใช้ function `summarise()` และ `group_by()` ในการหาว่าผู้ใช้เพศชายและเพศหญิง (`gender` มีค่าเป็น 1 และ 2 ตามลำดับ) มีจำนวน `reviews_count` โดยเฉลี่ย, จำนวน `n_photos` โดยเฉลี่ย, จำนวน `n_followers` โดยเฉลี่ย, ค่าเฉลี่ยและส่วนเบี่ยงเบนมาตรฐานของ `avg_rating` แตกต่างกันอย่างไร
- ไม่ต้องตั้งชื่อให้กับแต่ละคอลัมน์ และให้ R แสดงผลลัพธ์ออกมาได้เลยโดยไม่ต้องเก็บผลลัพธ์ไว้ในตัวแปรใดๆ

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")

user_new <- select(user, id, gender, reviews_count = n_reviews, contains("ratings"), n_photos, n_followers)

user_with_rating <- mutate(
	filter(user_new, reviews_count > 0), 
	avg_rating = (n_1_ratings + 2 * n_2_ratings + 3 * n_3_ratings + 4 * n_4_ratings + 5 * n_5_ratings) / 
	(n_1_ratings + n_2_ratings + n_3_ratings + n_4_ratings + n_5_ratings)
)
```

*** =sample_code
```{r}
# This to to summarise numbers of followers deparately by gender
summarise(group_by(user_with_rating, gender, n(n_followers))

# Use the code above as an example to complete your code here
summarise(
	group_by(..., ...), 
	mean(...), mean(...), mean(...), 
	mean(...), sd(...)
)
```

*** =solution
```{r}
# This to to summarise numbers of followers deparately by gender
summarise(group_by(user_with_rating, gender, n(n_followers))

# Use the code above as an example to complete your code here
summarise(
	group_by(user_with_rating, gender), 
	mean(reviews_count), mean(n_photos), mean(n_followers), 
	mean(avg_rating), sd(avg_rating)
)
```

*** =sct
```{r}
success_msg("Good Job!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:b1f530f06d
## การเรียงลำดับข้อมูลด้วย arrange() (1)

หากคุณต้องการจะเรียงลำดับข้อมูลเพื่อใช้ในการวิเคราะห์ หรือแม้แต่เพื่อจัดข้อมูลให้เป็นระเบียบมากขึ้น คุณสามารถใช้ function `arrange()` ในการจัดเรียงข้อมูลได้ เช่น
- คำสั่ง `arrange(user, id)` จะทำการเรียงข้อมูลในตัวแปร `user` ตามลำดับ `id`
- คำสั่ง `arrange(user, -n_followers, -n_reviews)` จะทำการเรียงข้อมูลในตัวแปร `w_user` โดยใช้คอลัมน์ `n_followers` และ `n_reviews` เป็นเกณฑ์ตามลำดับโดยเรียงจากมากไปหาน้อย (นั่นคือ ถ้ามีข้อมูลผู้ใช้งานคนไหนที่มีจำนวนผู้ติดตาม (`n_followers`) เท่ากัน R จะทำการพิจารณาคอลัมน์ `n_reviews` เป็นลำดับต่อไป)

*** =instructions
ในตอนนี้เรามีข้อมูล rating ทั้งหมดเก็บไว้ในตัวแปร `rating` ให้คุณทำตามคำสั่งดังต่อไปนี้:
- ใช้ function `arrange()` ในการเรียงลำดับ `restaurant` จาก `id` โดยให้เรียงจากน้อยไปหามาก แล้วเก็บค่าไว้ในตัวแปร `ascending_restaurant`
- ใช้ function `arrange()` ในการเรียงลำดับ `restaurant` จาก `id` โดยให้เรียงจากมากไปหาน้อย แล้วเก็บค่าไว้ในตัวแปร `descending_restaurant`
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `ascending_restaurant`
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `descending_restaurant`

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# `arrange` `restaurant` ascendingly by `id` and store result in `ascending_restaurant`
ascending_restaurant <- arrange(restaurant, ...)

# `arrange` `restaurant` descendingly by `id` and store result in `descending_restaurant`
... <- arrange(..., ...)

# print out the first 10 rows from `ascending_restaurant` and `descending_restaurant`
head(..., n = 10)
head(..., n = 10)
```

*** =solution
```{r}
# `arrange` `restaurant` ascendingly by `id` and store result in `ascending_restaurant`
ascending_restaurant <- arrange(restaurant, id)

# `arrange` `restaurant` descendingly by `id` and store result in `descending_restaurant`
descending_restaurant <- arrange(restaurant, id)

# print out the first 10 rows from `ascending_restaurant` and `descending_restaurant`
head(ascending_restaurant, n = 10)
head(descending_restaurant, n = 10)
```

*** =sct
```{r}
success_msg("Cool! Let's go to the next exercise")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:6bcc105e72
## การเรียงลำดับข้อมูลด้วย arrange() (2)

นอกจากการใช้เครื่องหมายลบ (`-`) แล้ว เรายังสามารถใช้คำสั่ง `desc()` กับตัวแปรที่เราต้องการให้เรียงลำดับจากมากไปหาน้อยได้ด้วยเช่นกัน คำสั่งดังต่อไปนี้:
- `arrange(user, desc(n_followers), desc(n_reviews))`
- `arrange(user, desc(n_followers, n_reviews))`
จะให้ผลเหมือนกันกับคำสั่งเรียงลำดับ `n_followers` และ `n_reviews` จากมากไปหาน้อยที่ได้กล่าวถึงไปในแบบฝึกหัดก่อนหน้านี้

*** =instructions
- ใช้ function `summarise()` ร่วมกับ `group_by()` ในการหา rating เฉลี่ยของร้านอาหารแต่ละร้าน (`reviewed_item_id`) พร้อมส่วนเบี่ยงเบนมาตรฐานจาก data frame `rating` ตั้งชื่อคอลัมน์ว่า `avg_rating` และ `sd_rating` ตามลำดับ จากนั้นเก็บผลลัพธ์ไว้ในตัวแปร `mean_rating`
- เรียงลำดับข้อมูลใน `mean_rating` ตามลำดับคอลัมน์ `avg_rating` และ `sd_rating` โดยเรียงจากมากไปหาน้อยcและน้อยไปหามากตามลำดับ แล้วเก็บผลลัพธ์ไว้ในตัวแปร `arranged_mean_rating`
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `arranged_mean_rating`

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
```

*** =sample_code
```{r}
# define a variable `mean_rating` here
mean_rating <- summarise(
	group_by(rating, ...),
	... = mean(...),
	... = sd(...)
)

# descendingly order `mean_rating` based on `avg_rating` and `sd_rating` and store result in `arranged_mean_rating`
arranged_mean_rating <- arrange(..., ..., ...)

# print out the first 10 row from `arranged_mean_rating`

```

*** =solution
```{r}
# define a variable `mean_rating` here
mean_rating <- summarise(
	group_by(rating, reviewed_item_id),
	avg_rating = mean(rating),
	sd_rating = sd(rating)
)

# descendingly order `mean_rating` based on `avg_rating` and `sd_rating` and store result in `arranged_mean_rating`
arranged_mean_rating <- arrange(mean_rating, desc(avg_rating), sd_rating)

# print out the first 10 row from `arranged_mean_rating`
head(arranged_mean_rating, n = 10)
```

*** =sct
```{r}
success_msg("Great Job!!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:d2e7c1321d
## การเชื่อมโยงข้อมูลต่างๆเข้าด้วยกัน (1)

การนำข้อมูลต่างๆมาเชื่อมโยงเข้าด้วยกัน จะช่วยให้เราเห็นความเกี่ยวข้องกันของข้อมูล อาจช่วยให้เราเข้าใจข้อมูลในภาพที่กว้างขึ้น ละเอียดขึ้น และเป็นจุดเริ่มต้นของการวิเคราะห์ข้อมูลให้เป็นประโยชน์ในลำดับต่อๆไปได้

เราสามารถใช้ function `inner_join()` มาช่วยในเชื่อม data frame เข้าด้วยกัน โดยคุณจะต้องทำการกำหนดว่าจะเชื่อม data frame เข้าด้วยกันโดยใช้คอลัมน์ใด เช่น

คำสั่ง `inner_join(t1, t2, by = c("a" = "b"))`: นำข้อมูลจาก `t1` และ `t2` มาเชื่อมกับผ่านคอลัมน์ `a` และ `b` โดยที่จะแสดงเฉพาะข้อมูลที่มีทั้งใน `t1` และ `t2`

*** =instructions
จากแบบฝึกหัดที่แล้ว เรามีข้อมูล rating เฉลี่ยของร้านอาหารแต่ละร้านเก็บไว้ในตัวแปร `mean_rating` แต่เรายังไม่มีข้อมูลชื่อร้านอาหารและอื่นๆ ในแบบฝึกหัดนี้เราจะลองนำข้อมูลดังกล่าวมาเชื่อมกับ data frame `restaurant` เพื่อให้มองเห็นข้อมูลในมุมที่ละเอียดขึ้น
- เลือกข้อมูลคอลัมน์ `id`, `name`, `price_range`, `category_id` จาก `restaurant` แล้วเก็บผลลัพธ์ไว้ในตัวแปร `restaurant_new`
- ใช้ function `inner_join()` ในการเชื่อม `restaurant_new` เข้ากับ `mean_rating` โดยใช้ `id` และ `reviewed_item_id` เป็นตัวเชื่อม (เนื่องจากทั้งคู่เป็นตัวแทนของ `id` ร้านอาหาร) เก็บผลลัพธ์ทีไ่ด้ไว้ในตัวแปร `restaurant_with_rating`
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `restaurant_with_rating`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")

mean_rating <- summarise(
	group_by(rating, reviewed_item_id), 
	avg_rating = mean(rating),
	sd_rating = sd(rating)
)
```

*** =sample_code
```{r}
# define `restaurant_new`
restaurant_new <- ...

# connect `restaurant_new` to `mean_rating` using `inner_join()` and store result in `restaurant_with_rating`
restaurant_with_rating <- inner_join(..., ..., by = c("..." = "..."))

# print out the first 10 row from `restaurant_with_rating`

```

*** =solution
```{r}
# define `restaurant_new`
restaurant_new <- select(restaurant, id, name, price_range, category_id)

# connect `restaurant_new` to `mean_rating` using `inner_join()` and store result in `restaurant_with_rating`
restaurant_with_rating <- inner_join(restaurant_new, mean_rating, by = c("id" = "reviewed_item_id"))

# print out the first 10 row from `restaurant_with_rating`
head(restaurant_with_rating, n = 10)
```

*** =sct
```{r}
success_msg("That's good! Let's move on to the next on the next exercise")
```



--- type:NormalExercise lang:r xp:100 skills:1 key:a1bb016a43
## การเชื่อมโยงข้อมูลต่างๆเข้าด้วยกัน (2)

ข้อจำกัดของ function `inner_join()` คือ การเชื่อมข้อมูลจะเกิดขึ้นกับแค่ข้อมูลที่มีอยู่ใน data frame ทั้ง 2 อันเท่านั้น ข้อมูลใดๆที่อยู่ใน data frame เพียงอันใดอันหนึ่งจะถูกตัดทิ้งไป

ในการแก้ปัญหาดังกล่าว เราสามารถใช้ function `left_join()` และ `right_join()` มาช่วยได้ ซึ่งมีคำอธิบายคร่าวๆดังต่อไปนี้:
- `left_join(t1, t2, by = c("a" = "b"))`: นำข้อมูลทุกแถวจาก `t1` มาเชื่อมกับ `t2` โดยใช้คอลัมน์ `a` จาก `t1` และคอลัมน์ `b` จาก `t2` เป็นตัวเชื่อม
- `right_join(t1, t2, by = c("a" = "b"))`: ตรงข้ามกับ `left_join()` คือจะเป็นการนำข้อมูลทุกแถวจาก `t2` มาเชื่อมกับ `t1` แทน

ในกรณีของ `left_join()` หากข้อมูลตัวใดที่มีอยู่ใน `t1` แต่ไม่อยู่ใน `t2` ทุกๆคอลัมน์จาก `t2` สำหรับข้อมูลตัวนั้นๆจะแสดงผลออกมาเป็น `NA`

--- may not include contents below

- `full_join(t1, t2, by = c("a" = "b"))`: นำข้อมูลทั้งหมดจากทั้ง `t1` และ `t2` มาเชื่อมกันผ่านคอลัมน์ `a` และ `b` โดยจะแสดงทุก combination ที่จะเป็นไปได้

ในกรณีที่ต้องการเชื่อมข้อมูลโดยมีคอลัมน์ที่ใช้เชื่อมมากกว่า 1 คอลัมน์ คุณสามารถกำหนด argument `by` ให้มีหลายเงื่อนไขได้เช่น `inner_join(t1, t2, by = c("a" = "b", "e" = "f"))` จะทำการเชื่อมข้อจาก `t1` และ `t2` เฉพาะข้อมูลในแถวที่มีคอลัมน์ `a` จาก `t1` เท่ากับ `b` จาก `t2` และ `e` จาก `t1` เท่ากับ `f` จาก `t2` เท่านั้น

ในกรณีที่ `t1` และ `t2` มีคอลัมน์ที่มีชื่อเดียวกัน R จะทำการเปลี่ยนชื่อคอลัมน์ที่ซ้ำกันให้ไม่เหมือนกัน เช่น ถ้าทั้ง `t1` และ `t2` มีคอลัมน์ `a` ทั้งคู่ คอลัมน์ หลังจากนำข้อมูลมาเชื่อมกัน คอลัมน์ `a` ใน `t1` จะถูกเปลี่ยนชื่อเป็น `a.x` ส่วนคอลัมน์ `a` ใน `t2` จะถูกเปลี่ยนชื่อเป็น `a.y`

*** =instructions
ให้คุณนำ `restaurant_new` มาเชื่อมกับ `mean_rating` เหมือนใแบบฝึกหัดที่แล้ว
- เลือกข้อมูลคอลัมน์ `id`, `name`, `price_range`, `category_id` จาก `restaurant` แล้วเก็บผลลัพธ์ไว้ในตัวแปร `restaurant_new`
- ใช้ function `left_join()` หรือ `right_join()` ในการเชื่อม `restaurant_new` เข้ากับ `mean_rating` โดยเราต้องการให้ผลลัพธ์มีข้อมูลของร้านอาหารครบทุกร้าน เก็บผลลัพธ์ทีไ่ด้ไว้ในตัวแปร `restaurant_with_rating` อย่าลืมว่าการนำ data frame มาเชื่อมกันควรมีการระบุคอลัมน์ที่จะใช้เป็นตัวเชื่อมด้วย
- เรียงลำดับข้อมูลใน `restaurant_with_rating` ตามคอลัมน์ `avg_rating` โดยเรียงจากมากไปหาน้อย และเก็บผลลัพธ์ไว้ใน `arranged_restaurant_with_rating`
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `arranged_restaurant_with_rating`
- คำนวณดูว่ามีร้านอาหารกี่เปอร์เซ็นต์ที่ไม่ได้รับการให้ rating เลย ให้ R แสดงคำตอบดังกล่าวออกมาด้วย

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")

mean_rating <- summarise(
	group_by(rating, reviewed_item_id), 
	avg_rating = mean(rating),
	sd_rating = sd(rating)
)
```

*** =sample_code
```{r}
# define `restaurant_new`
restaurant_new <- select(restaurant, id, name, price_range, category_id)

# connect `restaurant_new` to `mean_rating` using `left_join()` to get all restaurants info and store result in `restaurant_with_rating`
restaurant_with_rating <- ...

# ascendingly arrange `restaurant_with_rating` based on `avg_rating` and store result in `arranged_restaurant_with_rating`
arranged_restaurant_with_rating <- ...

# print out the first 10 row from `arranged_restaurant_with_rating`


# calculating proportion of restaurant with no rating and show the result
nrow(filter(arranged_restaurant_with_rating, ...)) / nrow(arranged_restaurant_with_rating)
```

*** =solution
```{r}
# define `restaurant_new`
restaurant_new <- select(restaurant, id, name, price_range, category_id)

# connect `restaurant_new` to `mean_rating` using `left_join()` to get all restaurants info and store result in `restaurant_with_rating`
restaurant_with_rating <- left_join(restaurant_new, mean_rating, by = c("id" = "reviewed_item_id"))

# ascendingly arrange `restaurant_with_rating` based on `avg_rating` and store result in `arranged_restaurant_with_rating`
arranged_restaurant_with_rating <- arrange(restaurant_with_rating, -avg_rating)

# print out the first 10 row from `arranged_restaurant_with_rating`
head(arranged_restaurant_with_rating, n = 10)

# calculating proportion of restaurant with no rating and show the result
nrow(filter(arranged_restaurant_with_rating, is.na(avg_rating))) / nrow(arranged_restaurant_with_rating)
```

*** =sct
```{r}

```
--- type:NormalExercise lang:r xp:100 skills:1 key:1aa667d382
## ทำความรู้จักกับ Pipes

ในการวิเคราะห์ข้อมูล เราสามารถใช้คำสั่งที่เรียกว่า pipes (`%>%`) เพื่อทำให้เขียน code ได้เป็นระเบียบและเป็นขั้นตอนมากขึ้น
โดยคำสั่ง pipes นี้จะประมวลผลเริ่มจากซ้ายไปขวา โดยใช้ตัวแปรหรือค่าใดๆก็ตามที่อยู่ด้านซ้ายของคำสั่งเป็น argument ตัวแรกของ function ทางด้านขวา เช่น:
- `user %>% ncol()` จะใช้ `user` เป็น `argument ของ function `ncol()` ที่อยู่ด้านขวาและจะแสดงผลเป็นจำนวนคอลัมน์ในตัวแปร `user`
- `rating %>% select(id, rating) %>% mutate(new_rating = rating + 2)` จะใช้ `rating` เป็น `argument ของ function `select()` ที่อยู่ด้านขวา จากนั้นจะใช้ผลลัพธ์ที่ได้จาก function `select()` เป็น argument ของ function `matate()` ทางด้านขวามือสุดต่อไป

*** =instructions
ให้คุณลองเขียนภาษา R โดยใช้ pipes (`%>%`) ในการสั่งให้ R แสดงค่าดังต่อไปนี้:
- `str(rating)`
- `nrow(rating)`
- `head(rating, n = 10)`
- `distinct(rating, rating)`
- `head(filter(select(user, id, gender, n_reviews, n_photos, n_followers), n_followers > 100))`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
```

*** =sample_code
```{r}
# do the same thing as `str(rating)` using pipes (`%>%`)
rating %>% str()

# do the same thing as `nrow(rating)` using pipes (`%>%`)


# do the same thing as `head(rating, n=10)` using pipes (`%>%`)


# do the same thing as `distinct(rating, rating)` using pipes (`%>%`)


# do the same thing as `head(filter(select(user, id, gender, n_reviews, n_photos, n_followers), n_followers > 100))` using pipes (`%>%`)
user %>% 
	select(..., ..., ..., ..., ...) %>% 
	filter(...) 
	%>% ...
```

*** =solution
```{r}
# do the same thing as `str(rating)` using pipes (`%>%`)
rating %>% str()

# do the same thing as `nrow(rating)` using pipes (`%>%`)
rating %>% nrow()

# do the same thing as `head(rating, n=10)` using pipes (`%>%`)
rating %>% head(n = 10)

# do the same thing as `distinct(rating, rating)` using pipes (`%>%`)
rating %>% distinct(rating)

# do the same thing as `head(filter(select(user, id, gender, n_reviews, n_photos, n_followers), n_followers > 100))` using pipes (`%>%`)
user %>% 
	select(id, gender, n_reviews, n_photos, n_followers) %>% 
	filter(n_followers > 100) %>% 
	head()
```

*** =sct
```{r}
success_msg("Good job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c2c15cb19a
## ทำความรู้จักกับ Pipes (2)

ในกรณีที่เราต้องการจะเก็บผลลัพธ์จากการใช้ pipes ไว้ในตัวแปร เราก็สามารถทำได้โดยการใช้เครื่องหมายกำหนดค่า `<-` ตามปกติ เช่น `a <- 1:5 %>% sum()`

และในกรณีที่เราต้องการจะใช้ข้อมูลที่อยู่ด้านซ้ายของคำสั่ง `%>%` เป็น argument ในตำแหน่งอื่นๆที่ไม่ใช่ตำแหน่งแรก เราก็สามารถใช้ `.` เป็นตัวแทนตำแหน่งที่เราต้องการได้ เช่น
`8 %>% head(user, n = .)` จะทำการนำ `8` ไปใส่เป็น argument ตรงตำแหน่ง `.` แทนที่จะเป็นตำแหน่งแรก

*** =instructions
เราได้สร้างตัวแปร `restaurant` และ `rating` ไว้ให้คุณใน workspace แล้ว ให้ปฏิบัติตามคำสั่งต่อไปนี้ โดยใช้ pipes (`%>%`):
- ใช้ function `summarise()` ร่วมกับ `group_by()` ในการหา rating เฉลี่ยของร้านอาหารแต่ละร้าน (`reviewed_item_id`) พร้อมส่วนเบี่ยงเบนมาตรฐาน ตั้งชื่อคอลัมน์ใหม่ว่า `avg_rating` และ `sd_rating` ตามลำดับ เก็บผลลัพธ์ไว้ในตัวแปร `mean_rating`
- ใช้ function `inner_join()` ในการเชื่อม `restaurant` เข้ากับผลลัพธ์ในคำสั่งที่แล้ว โดยใช้คอลัมน์ `reviewed_item_id` จาก `rating` เป็นตัวเชื่อมกับ `id` จาก `restaurant` แล้วเก็บผลลัพธ์ไว้ในตัวแปร `restaurant_with_rating`
- เลือกข้อมูลจากผลลัพธ์ในคำสั่งที่แล้ว โดยเลือกมาแต่คอลัมน์ `reviewed_item_id`, `name`, `price_range`, `avg_rating` และ `sd_rating` เก็บผลลัพธ์ไว้ในตัวแปร `temp_result`
- เรียงลำดับ `result` ตามคอลัมน์ `avg_rating` โดยเรียงจากร้านอาหารที่มีคะแนนเฉลี่ยมากไปถึงน้อย และเก็บผลลัพธ์ไว้ในตัวแปร `final_result`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
```

*** =sample_code
```{r}
# your code here
mean_rating <- rating %>% 
	group_by(...) %>% 
	summarise(avg_rating = ..., sd_rating = ...)

restaurant_with_rating <- ... %>% inner_join(..., by = ...)

temp_result <- ... %>% select(..., ..., ..., ..., ...)

final_result <- ... %>% arange(...)
```

*** =solution
```{r}
# your code here
mean_rating <- rating %>% 
	group_by(reviewed_item_id) %>% 
	summarise(avg_rating = mean(rating), sd_rating = sd(rating))

restaurant_with_rating <- mean_rating %>% inner_join(restaurant, by = c("reviewed_item_id" = "id"))

temp_result <- restaurant_with_rating %>% select(reviewed_item_id, name, price_range, avg_rating, sd_rating)

final_result <- temp_result %>% arange(-avg_rating)
```

*** =sct
```{r}
success_msg("Cool!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:b2a93e992b
## ทำความรู้จักกับ Pipes (3)

การใช้ pipes จะช่วยให้เราสามารถเขียน code ต่อกันได้อย่างเป็นระเบียบและเป็นลำดับ โดยอาจไม่จำเป็นต้องสร้างตัวแปรขึ้นมาเก็บผลลัพธ์ระหว่างทางก็ได้ เช่น

คำสั่ง `restaurant %>% filter(wifi == 1) %>% group_by(price_range) %>% summarise(verified_rate = mean(verified_info))` จะทำการวิเคราะห์ข้อมูลอย่างเป็นขั้นเป็นตอน เริ่มจากการกรองข้อมูล จัดกลุ่มข้อมูล และสรุปข้อมูล ซึ่งทั้งหมดนี้สามารถทำได้ในคำสั่งเดียวโดยไม่ต้องพึ่งการสร้างตัวแปรใดๆ

*** =instructions
เราได้สร้างตัวแปร `restaurant` และ `rating` ไว้ให้คุณใน workspace แล้ว ให้ทำตามแบบฝึกหัดที่แล้ว โดยใช้ pipes (`%>%`) และเขียนทุกอย่างต่อกันตั้งแต่ต้นจนจบในคำสั่งเดียว:
- ใช้ function `summarise()` ร่วมกับ `group_by()` ในการหา rating เฉลี่ยของร้านอาหารแต่ละร้าน (`reviewed_item_id`) พร้อมส่วนเบี่ยงเบนมาตรฐาน ตั้งชื่อคอลัมน์ใหม่ว่า `avg_rating` และ `sd_rating` ตามลำดับ
- ใช้ function `inner_join()` ในการเชื่อม `restaurant` เข้ากับผลลัพธ์ในคำสั่งที่แล้ว โดยใช้คอลัมน์ `reviewed_item_id` จาก `rating` เป็นตัวเชื่อมกับ `id` จาก `restaurant`
- เลือกข้อมูลจากผลลัพธ์ในคำสั่งที่แล้ว โดยเลือกมาแต่คอลัมน์ `id`, `name`, `price_range`, `avg_rating` และ `sd_rating`
- เรียงลำดับ `result` ตามคอลัมน์ `avg_rating` โดยเรียงจากร้านอาหารที่มีคะแนนเฉลี่ยมากไปถึงน้อย
- สุดท้าย แสดงผลลัพธ์ 10 แถวแรกออกมา

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
```

*** =sample_code
```{r}
# create a chain of pipes code here
rating %>% 
	group_by(...) %>% 
	summarise(avg_rating = ..., sd_rating = ...) %>%
	inner_join(..., by = ...) %>%
	select(...) %>% 
	arrange(...) %>%
	head(...)
```

*** =solution
```{r}
# create a chain of pipes code here
rating %>% 
	group_by(reviewed_item_id) %>% 
	summarise(avg_rating = mean(rating), sd_rating = sd(rating)) %>%
	inner_join(restaurant, by = c("reviewed_item_id" = "id")) %>%
	select(reviewed_item_id, name, price_range, avg_rating, sd_rating) %>% 
	arrange(-avg_rating) %>%
	head(n = 10)
```

*** =sct
```{r}
success_msg("Cool!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:c916ed8e54
## เรามาวิเคราะห์คะแนนเฉลี่ยของร้านอาหารแต่ละกลุ่มกัน!

หลังจากที่คุณได้เรียนรู้กับ function พื้นฐานต่างๆของ package Dplyr ไปแล้ว ถึงเวลาแล้วที่เราจะให้คุณลองลงมือดึงข้อมูลเพื่อนำมาวิเคราะห์ด้วยตัวเอง!

เพื่อเป็นการทบทวน รายละเอียดการใช้งานของแต่ละ function พื้นฐาน มีดังต่อไปนี้:
- `select()` ใช้เลือกข้อมูลคอลัมน์ต่างๆจาก data frame
- `distinct()` ใช้ในการดึงเฉพาะข้อมูลที่มีค่าไม่ซ้ำกัน สามารถใช้ในการสำรวจข้อมูลคร่าวๆได้ ว่ามีค่าที่เป็นไปได้ทั้งหมดกี่แบบ
- `arrange()` ใช้ในการเรียงลำดับข้อมูล คุณสามารถเรียงจากค่ามากไปหาน้อยได้โดยการใส่เครื่องหมาย `-` หรือ `desc()`
- `filter()` ใช้ในการกรองข้อมูลเพื่อดึงเฉพาะข้อมูลที่ตรงตามเงื่อนไขที่คุณต้องการออกมา
- `summarise()` ใช้ในการสรุปข้อมูลจาก data frame ของคุณ สามารถใช้งานกับ function การคำนวณอื่นๆได้อย่างอิสระ เช่น `n()`, `n_distinct()`, `sum()`, `mean()` และ `sd()` เป็นต้น
- `group_by` สามารถใช้ร่วมกับ function `summarise()` เพื่อแบ่งข้อมูลออกเป็นกลุ่มตามค่าในคอลัมน์ได้
- `left_join()`, `right_join()`, `inner_join()`, `full_join()` ใช้ในการนำข้อมูลมาเชื่อมโยงกันตามค่าในคอลัมน์ที่คุณกำหนด
- `%>%` ช่วยให้คุณสามารถเขียน code ได้อย่างเป็นขั้นเป็นตอนมากขึ้นจากซ้ายไปขวา ทำให้ code ของคุณเป็นระเบียบมากขึ้น และอาจช่วยให้คุณทำงานง่ายขึ้นด้วย!

*** =instructions
ตอนนี้เรามีข้อมูล `restaurant`, `rating` และ `category` อยู่ใน workspace ให้คุณนำ data frame ทั้ง 3 อันมาวิเคราะห์ข้อมูลร่วมกันตามนี้:
- เราต้องการข้อมูลสรุป จำนวนร้านอาหาร, จำนวนครั้งที่มีการให้คะแนน (`rating`), คะแนนเฉลี่ย, ส่วนเบี่ยงเบนมาตรฐานของคะแนน โดยแบ่งตามแต่ละ category
- ให้ดึงข้อมูลมาเฉพาะร้านที่เป็นร้านอาหารจริงๆเท่านั้น (`domain_id` ใน `restaurant` มีค่าเป็น 1)
- ไม่ต้องสนใจร้านอาหารที่ไม่มี `rating`
- คัดกรองเฉพาะ category ที่มีจำนวนครั้ง `rating` มากกว่าหรือเท่ากับ 5 ครั้ง
- เรียงลำดับผลลัพธ์สุดท้ายตามคะแนนเฉลี่ยจากมากไปหาน้อย หาก category ใดมีคะแนนเฉลี่ยเท่ากัน ให้เรียงตามจำนวนครั้ง `rating` จากมากไปน้อยเป็นลำดับต่อไป
- ผลลัพธ์สุดท้ายควรประกอบด้วย `category_id` (รหัส category), `category_name` (ชื่อ category), `n_restaurants` (จำนวนร้านอาหารใน category นั้นๆ), `n_ratings` (จำนวน rating ใน category นั้นๆ), `avg_rating` (คะแนนเฉลี่ยของ category), `sd_rating` (ส่วนเบี่ยงเบนมาตรฐานของคะแนนในแต่ละ category)
- เก็บผลลัพธ์ไว้ในตัวแปร `category_with_rating` พร้อมทั้งให้ R แสดงผลลัพธ์ออกมาด้วย
- คุณสามารถทำแบบฝึกหัดนี้โดยการทำไปทีละขั้นตอน หรือจะเขียน code ให้ต่อกันหมดในคำสั่งเดียวก็ได้

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
category <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/category.tsv")
```

*** =sample_code
```{r}

```

*** =solution
```{r}
category_with_rating <-
	restaurant %>% 
	filter(domain_id == 1) %>% 
	select(id, category_id) %>%
	inner_join(rating, by = c("id" = "reviewed_item_id")) %>% 
	group_by(category_id) %>% 
	summarise(
		n_restaurants = n_distinct(id), 
		n_ratings = n(), 
		avg_rating = mean(rating), 
		sd_rating = sd(rating)) %>%
	inner_join(category, by = c("category_id" = "id")) %>% 
	select(category_id, category_name = name, n_restaurants, n_ratings, avg_rating, sd_rating) %>% 
	filter(n_ratings >= 5) %>% 
	arrange(desc(avg_rating))

category_with_rating

```

*** =sct
```{r}
success_msg("Wonderful! Now you've finished chapter 1!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:a3b42e7270
## <<<New Exercise>>>


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

--- type:NormalExercise lang:r xp:100 skills:1 key:733ce43a78
## <<<New Exercise>>>


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
