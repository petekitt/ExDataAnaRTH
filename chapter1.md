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
success_msg("Good! Now you can see that we actually have 3 genders in our user data - 1 for male, 2 for female, and 0 for the unspecified. Now, we will move to the next exercise!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:0783bbda27
## คัดกรองข้อมูลตามเงื่อนไขและสร้างคอลัมน์ใหม่ในข้อมูลด้วย filter() และ mutate()!

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
w_user_new <- select(w_user, id, gender, reviews_count = n_reviews, contains("ratings"), n_photos, n_followers)
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
## การสรุปข้อมูลด้วย summarise() และ group_by()

ในแบบฝึกหัดที่แล้ว เราได้ทำการคัดกรองข้อมูลเฉพาะผู้ใช้ที่มีการเขียนรีวิวลงบนเว็บไซต์ wongnai และคำนวณคะแนนเฉลี่ยที่ผู้ใช้แต่ละคนจะให้กับร้านอาหารต่างๆ แล้วเก็บไว้ในตัวแปร `mutate_w_user`
คราวนี้เราจะมาลองวิเคราะห์ข้อมูลกันให้ลึกมากขึ้นโดยใช้ function `summarise()` และ `group_by()`

function `summarise()` จะช่วยให้เราสามารถสรุปข้อมูลได้ตามที่เราต้องการ โดยขึ้นอยู่กับว่าเราใส่อะไรลงไปเป็น argument ของ function `summarise()` ยกตัวอย่างเช่น:
- คำสั่ง `summarise(w_user, n())` จะทำการสรุปข้อมูลให้เราว่าข้อมูลใน data frame `w_user` มีทั้งหมดกี่แถว (หรือก็คือมีผู้ใช้จำนวนกี่คน)
- คำสั่ง `summarise(w_user, mean(n_reviews), sum(n_3_ratings), median(n_followers))` จะแสดงผลลัพธ์เป็นจำนวน review เฉลี่ยต่อผู้ใช้ 1 คน, จำนวน rating ระดับ 3 ทั้งหมดที่ผู้ใช้ใน `w_user` ได้ให้กับร้านอาหาร และค่ามัธยฐานของจำนวนผู้ติดตามของผู้ใช้แต่ละคนใน `w_user`
คุณสามารถลองเล่นกับ function `summarise()` นี้ได้โดยการนำ code เหล่านี้ไปพิมพ์ใน Console และลองดูว่าผลเป็นอย่างไร

ในการวิเคราะห์ข้อมูลในการทำงานจริง เราอาจจะอยากทำการวิเคราะห์ให้ลึกลงไปโดยแบ่งข้อมูลที่เรามีตามกลุ่มต่างๆด้วย ซึ่ง function `group_by()` จะสามารถช่วยเราได้ในจุดนี้
หากคุณลองพิมพ์คำสั่ง `summarise(group_by(w_restaurant, price_range), n(), sum(verified_info), verified_rate = sum(verified_info) / n())` ลงไปใน Console สิ่งที่คุณจะเห็นจากผลลัพธ์คือการสรุปข้อมูลของจำนวนร้านอาหารทั้งหมด, จำนวนร้านที่ได้รับการยืนยันข้อมูลแล้ว และอัตราการยืนยันข้อมูล โดยแบ่งกลุ่มตาม `price_range`

ซึ่งจากผลลัพธ์ดังกล่าว คุณจะเห็นได้ว่าร้านอาหารที่มี price_range จัดอยู่ในระดับสูงกว่า(ราคาอาหารแพงกว่า) มีแนวโน้มที่จะได้รับการยืนยันข้อมูลมากกว่าอย่างชัดเจน

*** =instructions
ให้คุณทำการวิเคราะห์ข้อมูลผู้ใช้ต่อโดยใช้ข้อมูลจากตัวแปร `mutate_w_user` ที่ถูกเตรียมไว้ให้ใน workspace แล้ว
- ทำการสรุปข้อมูลโดยใช้ function `summarise()` และ `group_by()` ในการหาว่าผู้ใช้เพศชายและเพศหญิง (`gender` มีค่าเป็น 1 และ 2 ตามลำดับ) มีจำนวน `reviews_count`, จำนวน rating ทั้งหมด (ผลรวมตั้งแต่ `n_1_ratings` ไปจนถึง `n_5_ratings`), จำนวน `n_photos` และจำนวน `n_followers` โดยเฉลี่ยแตกต่างกันอย่างไร และมีค่าเฉลี่ยและส่วนเบี่ยงเบนมาตรฐานของ `avg_rating` ต่างกันอย่างไร
- คุณควรใช้ function `mean()` และ `sd()` ร่วมกับ `summarise()` ในการหาค่าเฉลี่ยและส่วนเบี่ยงเบนมาตรฐาน
- ไม่ต้องตั้งชื่อให้กับแต่ละคอลัมน์ และให้ R แสดงผลลัพธ์ออกมาได้เลยโดยไม่ต้องเก็บผลลัพธ์ไว้ในตัวแปรใดๆ

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')
w_user <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_user.tsv')
w_user_new <- select(w_user, id, gender, reviews_count = n_reviews, contains("ratings"), n_photos, n_followers)
mutate_w_user <- mutate(filter(w_user_new, reviews_count > 0), avg_rating = (n_1_ratings + 2*n_2_ratings + 3*n_3_ratings +
	4*n_4_ratings + 5*n_5_ratings) / (n_1_ratings + n_2_ratings + n_3_ratings + n_4_ratings + n_5_ratings))
```

*** =sample_code
```{r}
# This will return summary information about average amount of total level 1 rating through level 3 rating per 1 user
summarise(w_user, mean(n_1_ratings + n_2_ratings + n_3_ratings))

# Your code here

```

*** =solution
```{r}
# This will return summary information about average amount of total level 1 rating through level 3 rating per 1 user
summarise(w_user, mean(n_1_ratings + n_2_ratings + n_3_ratings))

# Your code here
summarise(group_by(mutate_w_user, gender), mean(reviews_count), mean(n_1_ratings + n_2_ratings + n_3_ratings + n_4_ratings + n_5_ratings), mean(n_photos), mean(n_followers), mean(avg_rating), sd(avg_rating))
```

*** =sct
```{r}
success_msg("Well done!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:b1f530f06d
## การเรียงลำดับข้อมูลด้วย arrange()

หากคุณต้องการจะเรียงลำดับข้อมูลเพื่อใช้ในการวิเคราะห์ หรือแม้แต่เพื่อจัดข้อมูลให้เป็นระเบียบมากขึ้น คุณสามารถใช้ function `arrange()` ในการจัดเรียงข้อมูลได้ เช่น
- คำสั่ง `arrange(w_user, id)` จะทำการเรียงข้อมูลในตัวแปร `w_user` ตามลำดับ `id`
- คำสั่ง `arrange(w_user, -n_followers, -n_reviews)` จะทำการเรียงข้อมูลในตัวแปร `w_user` โดยใช้คอลัมน์ `n_followers` และ `n_reviews` เป็นเกณฑ์ตามลำดับโดยเรียงจากมากไปหาน้อย (นั่นคือ ถ้ามีข้อมูลผู้ใช้งานคนไหนที่มีจำนวนผู้ติดตาม (`n_followers`) เท่ากัน R จะทำการพิจารณาคอลัมน์ `n_reviews` เป็นลำดับต่อไป) 

สังเกตได้ว่า function `arrange()` จะทำหน้าที่คล้ายกับ function `order()` ในภาษา R ปกติ แต่นอกจากการใส่เครื่องหมายลบ (`-`) เพื่อบอกให้ R เรียงลำดับข้อมูลจากมากไปหาน้อยแล้ว เรายังสามารถใช้คำสั่ง `desc()` กับตัวแปรที่เราต้องการให้เรียงลำดับจากมากไปหาน้อยได้ด้วยเช่นกัน นั่นคือ คำสั่งดังต่อไปนี้:
- `arrange(w_user, desc(n_followers), desc(n_reviews))`
- `arrange(w_user, desc(n_followers, n_reviews))`
จะให้ผลเหมือนกันกับคำสั่งเรียงลำดับ `n_followers` และ `n_reviews` จากมากไปหาน้อยที่ได้กล่าวถึงไปในตอนแรก

*** =instructions
ในตอนนี้เรามีข้อมูล rating ทั้งหมดเก็บไว้ในตัวแปร `w_rating` ให้คุณทำตามคำสั่งดังต่อไปนี้:
- ใช้ function `summarise()` ร่วมกับ `group_by()` ในการหา rating เฉลี่ยของร้านอาหารแต่ละร้าน (`reviewed_item_id`) พร้อมส่วนเบี่ยงเบนมาตรฐาน แล้วเรียงลำดับข้อมูลตาม rating เฉลี่ยจากมากไปหาน้อย จากนั้นเก็บผลลัพธ์ไว้ในตัวแปร `avg_restaurant_rating`
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `avg_restaurant_rating`
- นับจำนวน rating ต่างๆตั้งแต่ 1 ถึง 5 (คอลัมน์ `rating`) รวมถึงนับจำนวน เรียงลำดับข้อมูลตามคอลัมน์ `rating` จากน้อยไปมาก จากนั้นเก็บผลลัพธ์ไว้ในตัวแปร `rating_count`
- สั่งให้ R แสดงค่าของ `rating_count`

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_user <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_user.tsv')
w_rating <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_rating.tsv')
```

*** =sample_code
```{r}
# define `avg_restaurant_rating` here and print the first 10 rows out


# define `rating_count` here and print it out

```

*** =solution
```{r}
# define `avg_restaurant_rating` here and print the first 10 rows out
avg_restaurant_rating <- arrange(summarise(group_by(w_rating, reviewed_item_id), avg_rating = mean(rating)), -avg_rating)
head(avg_restaurant_rating, n=10)

# define `rating_count` here and print it out
rating_count <- arrange(summarise(group_by(w_rating, rating), n()), rating)
rating_count
```

*** =sct
```{r}
success_msg("Cool! Let's go to the next exercise")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:d2e7c1321d
## เรามาเชื่อมข้อมูลต่างๆเข้าด้วยกันดีกว่า!

แน่นอนว่าการนำข้อมูลจากหลายๆ data frame มาหาจุดเชื่อมโยงกันนั้นเป็นเรื่องที่หลีกเลี่ยงไม่ได้

package Dplyr มี function ที่จะช่วยในการนำ data frame ต่างๆมาเชื่อมต่อกัน ได้แก่ `left_join()`, `right_join()`, `inner_join()`, `full_join()` และ `full_join()` ซึ่งมีคำอธิบายคร่าวๆดังต่อไปนี้:
- `left_join(x, y, by=c('a'='b'))`: นำข้อมูลทุกแถวจาก `x` มาเชื่อมกับ `y` โดยใช้คอลัมน์ `a` จาก `x` และคอลัมน์ `b` จาก `y` เป็นตัวเชื่อม
- `right_join(x, y, by=c('a'='b'))`: ตรงข้ามกับ `left_join()` คือจะเป็นการนำข้อมูลทุกแถวจาก `y` มาเชื่อมกับ `x` แทน
- `inner_join(x, y, by=c('a'='b'))`: นำข้อมูลจาก `x` และ `y` มาเชื่อมกับผ่านคอลัมน์ `a` และ `b` โดยที่จะแสดงเฉพาะข้อมูลที่มีทั้งใน `x` และ `y`
- `full_join(x, y, by=c('a'='b'))`: นำข้อมูลทั้งหมดจากทั้ง `x` และ `y` มาเชื่อมกันผ่านคอลัมน์ `a` และ `b` โดยจะแสดงทุก combination ที่จะเป็นไปได้

ในกรณีที่ต้องการเชื่อมข้อมูลโดยมีคอลัมน์ที่ใช้เชื่อม

*** =instructions
จากแบบฝึกหัดที่แล้ว เรามีข้อมูล rating เฉลี่ยของร้านอาหารแต่ละร้านจาก data frame ในตัวแปร `avg_restaurant_rating` ในแบบฝึกหัดนี้เราจะลองนำข้อมูลดังกล่าวมาเชื่อมกับ data frame ในตัวแปร `w_restaurant` เพื่อทำการวิเคราะห์ต่อไป
- เลือกข้อมูลคอลัมน์ `id`, `name`, `price_range`, `category_id` จาก `w_restaurant` แล้วเก็บผลลัพธ์ไว้ในตัวแปร `w_restaurant_new`
- ใช้ function `left_join()` หรือ `right_join()` ในการเชื่อม `w_restaurant_new` เข้ากับ `avg_restaurant_rating` โดยเราต้องการให้ผลลัพธ์มีข้อมูลของร้านอาหารครบทุกร้าน เรียงข้อมูลตาม `id` ของร้านอาหาร เก็บผลลัพธ์ทีไ่ด้ไว้ในตัวแปร `w_restaurant_with_rating` อย่าลืมว่าการนำ data frame มาเชื่อมกันควรมีการระบุคอลัมน์ที่จะใช้เป็นตัวเชื่อมด้วย
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `w_restaurant_with_rating`
- คำนวณดูว่ามีร้านอาหารกี่เปอร์เซ็นต์ที่ไม่ได้รับการให้ rating เลย เก็บคำตอบไว้ในตัวแปร `no_rating_proportion` แล้วสั่งให้ R แสดงคำตอบดังกล่าวออกมาด้วย

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_restaurant <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_restaurant.tsv', encoding='UTF-8')
w_rating <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_rating.tsv')
avg_restaurant_rating <- arrange(summarise(group_by(w_rating, reviewed_item_id), avg_rating = mean(rating)), -avg_rating)
head(avg_restaurant_rating, n=10)
```

*** =sample_code
```{r}
# link `w_restaurant_new` to `avg_restaurant_rating` by using `left_join()` or `right_join()`
w_restaurant_mew <- 

# calculate the no-rating proportion here
no_rating_proportion <- 
```

*** =solution
```{r}
# link `w_restaurant_new` to `avg_restaurant_rating` by using `left_join()` or `right_join()`
w_restaurant_new <- select(w_restaurant, id, name, price_range, category_id)
w_restaurant_with_rating <- arrange(left_join(w_restaurant_new, avg_restaurant_rating, by=c('id'='reviewed_item_id')), id)
head(w_restaurant_with_rating, n=10)

# calculate the no-rating proportion here
no_rating_proportion <- nrow(filter(w_restaurant_with_rating, is.na(avg_rating))) / nrow(w_restaurant_with_rating)
no_rating_proportion
```

*** =sct
```{r}
success_msg("That's good! Let's move on to the next on the next exercise")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:1aa667d382
## ทำความรู้จักกับ Pipes

ในการวิเคราะห์ข้อมูล เราสามารถใช้คำสั่งที่เรียกว่า pipes (`%>%`) เพื่อทำให้เขียน code ได้เป็นระเบียบและเป็นขั้นตอนมากขึ้น
โดยคำสั่ง pipes นี้จะประมวลผลเริ่มจากซ้ายไปขวา โดยใช้ตัวแปรหรือค่าใดๆก็ตามที่อยู่ด้านซ้ายของคำสั่งเป็น argument ตัวแรกของ function ทางด้านขวา เช่น:
- `w_user %>% ncol()` จะใช้ `w_user` เป็น `argument ของ function `ncol()` ที่อยู่ด้านขวาและจะแสดงผลเป็นจำนวนคอลัมน์ในตัวแปร `w_user`
- `w_rating %>% select(id, rating) %>% mutate(new_rating = rating + 2)` จะใช้ `w_rating` เป็น `argument ของ function `select()` ที่อยู่ด้านขวา จากนั้นจะใช้ผลลัพธ์ที่ได้จาก function `select()` เป็น argument ของ function `matate()` ทางด้านขวามือสุดต่อไป

*** =instructions
ให้คุณลองเขียนภาษา R โดยใช้ pipes (`%>%`) ในการสั่งให้ R แสดงค่าดังต่อไปนี้:
- `str(w_rating)`
- `nrow(w_rating)`
- `head(w_rating, n=10)`
- `distinct(w_rating, rating)`
- `head(filter(select(w_user, id, gender, n_reviews, n_photos, n_followers), n_followers > 100))`

*** =hint

*** =pre_exercise_code
```{r}
library('dplyr')
w_user <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_user.tsv')
w_rating <- read.delim('http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/w_rating.tsv')
```

*** =sample_code
```{r}
# do the same thing as `str(w_rating)` using pipes (`%>%`)
w_rating %>% str()

# do the same thing as `nrow(w_rating)` using pipes (`%>%`)


# do the same thing as `head(w_rating, n=10)` using pipes (`%>%`)


# do the same thing as `distinct(w_rating, rating)` using pipes (`%>%`)


# do the same thing as `head(filter(select(w_user, id, gender, n_reviews, n_photos, n_followers), n_followers > 100))` using pipes (`%>%`)

```

*** =solution
```{r}
# do the same thing as `str(w_rating)` using pipes (`%>%`)
w_rating %>% str()

# do the same thing as `nrow(w_rating)` using pipes (`%>%`)
w_rating %>% nrow()

# do the same thing as `head(w_rating, n=10)` using pipes (`%>%`)
w_rating %>% head(n=10)

# do the same thing as `distinct(w_rating, rating)` using pipes (`%>%`)
w_rating %>% distinct(rating)

# do the same thing as `head(filter(select(w_user, id, gender, n_reviews, n_photos, n_followers), n_followers > 100))` using pipes (`%>%`)
w_user %>% select(id, gender, n_reviews, n_photos, n_followers) %>% filter(n_followers > 100) %>% head()
```

*** =sct
```{r}
success_msg("Good job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c2c15cb19a
## ทำความรู้จักกับ Pipes (2)

เราจะมาลองใช้ pipes ในระดับที่ยากขึ้นไปอีกหน่อย
ในกรณีที่เราต้องการจะใช้ข้อมูลที่อยู่ด้านซ้ายของคำสั่ง `%>%` เป็น argument ในตำแหน่งอื่นๆที่ไม่ใช่ตำแหน่งแรก เราก็สามารถใช้ `.` เป็นตัวแทนตำแหน่งที่เราต้องการได้ เช่น
- `8 %>% head(w_user, n=.)` จะทำการนำ `8` ไปใส่เป็น argument ตรงตำแหน่ง `.` แทนที่จะเป็นตำแหน่งแรก

*** =instructions
เราได้สร้างตัวแปร `w_restaurant` และ `w_rating` ไว้ให้คุณใน workspace แล้ว ให้ปฏิบัติตามคำสั่งต่อไปนี้ โดยใช้ pipes (`%>%`):
- ใช้ function `summarise()` ร่วมกับ `group_by()` ในการหา rating เฉลี่ยของร้านอาหารแต่ละร้าน (`reviewed_item_id`) พร้อมส่วนเบี่ยงเบนมาตรฐาน ตั้งชื่อคอลัมน์ใหม่ว่า `avg_rating` และ `sd_rating` ตามลำดับ
- ใช้ function `inner_join()` ในการเชื่อม `w_restaurant` เข้ากับผลลัพธ์ในคำสั่งที่แล้ว โดยเราต้องการให้ผลลัพธ์มีข้อมูลของร้านอาหารครบทุกร้าน เรียงข้อมูลตาม `id` ของร้านอาหาร
- เลือกข้อมูลจากผลลัพธ์ในคำสั่งที่แล้ว โดยเลือกมาแต่คอลัมน์ `id`, `name`, `price_range`, `avg_rating` และ `sd_rating`
- เก็บผลลัพธ์ในคำสั่งที่แล้วไว้ในตัวแปร `result`
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `result`

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
success_msg("Cool!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c916ed8e54
## pending 1


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

--- type:NormalExercise lang:r xp:100 skills:1 key:5157e559cb
## pending 2


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
