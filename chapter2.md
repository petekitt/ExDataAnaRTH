---
title meta  : บทที่ 2
title       : Exploratory Data Analysis on One Variable
description : Insert the chapter description here
--- type:NormalExercise lang:r xp:100 skills:1 key:f055d83ad0
## เรามาลองสำรวจข้อมูลผู้ใช้งานกันอีกหน่อย (1)

ทวนความจำเรื่องการใช้ factor เพื่อเปลี่ยนแปลงรูปแบบข้อมูลให้เหมาะสมกับการวิเคราะห์ข้อมูลประเภท categorical มากขึ้น

*** =instructions
ในตอนนี้เรามี data frame `user` ซึ่งเก็บข้อมูลของผู้ใช้งานเว็บไซต์อยู่ใน workspace ให้คุณกด `submit` เพื่อลองดูผลของการใช้ function `factor` กับคอลัมน์ `gender` จาก data frame `user`

*** =hint

*** =pre_exercise_code
```{r}
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# specified gender data type to be categorical data or `factor`
gender_factor <- factor(user$gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))

# display some of the first rows from `gender_factor`
head(gender_factor)

# display a summary as well
summary(gender_factor)
```

*** =solution
```{r}
# specified gender data type to be categorical data or `factor`
gender_factor <- factor(user$gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))

# display some of the first rows from `gender_factor`
head(gender_factor)

# display a summary as well
summary(gender_factor)
```

*** =sct
```{r}
success_msg("Good Job!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:4d53216bc3
## เรามาลองสำรวจข้อมูลผู้ใช้งานกันอีกหน่อย (2)

นอกจากการใช้ `summary` เพื่อสรุปข้อมูลแล้ว หากเราต้องการนับจำนวนข้อมูล categorical อย่างง่ายๆใน R เราก็สามารถใช้ function `table` ช่วยได้

*** =instructions
ต่อจากแบบฝึกหัดที่แล้ว ให้คุณลองใช้ function `table` กับตัวแปร `gender_factor`

*** =hint

*** =pre_exercise_code
```{r}
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
gender_factor <- factor(user$gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))
head(gender_factor)
summary(gender_factor)
```

*** =sample_code
```{r}
# try using `table` function to the variable `gender_factor`

```

*** =solution
```{r}
# try using `table` function to the variable `gender_factor`
table(gender_factor)
```

*** =sct
```{r}
success_msg("You might see that `table` is another good way to help you count for the number of each possible categorical value in R. Now, we get to the next exercise!")
```


--- type:NormalExercise lang:r xp:100 skills:1 key:a1a6a34df8
## เตรียมข้อมูลก่อนการวิเคราะห์ต่อไป

เราสามารภนำ fucntion ที่ได้กล่าวถึงไปในแบบฝึกหัดก่อนๆมาใช้ร่วมกับ package `dplyr` ในการจัดเตรียมรูปแบบข้อมูลให้เหมาะสมกับการวิเคราะห์ต่อไปได้

*** =instructions
ให้คุณลองใช้ fucntion `factor` ร่วมกับความรู้เกี่ยวกับ function ต่างๆรวมถึง pipes (`%>%`) ใน package `dplyr` เพื่อจัดรูปแบบข้อมูลให้เหมาะสมแล้วเก็บค่าไว้ในตัวแปร `gender_summary` โดยการนำคำสั่งเหล่านี้ไปเติมในช่องว่างให้สมบูรณ์

- ใช้คำสั่ง `mutate(gender = factor(gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown')))` เพื่อทำการแปลงค่าคอลัมน์ `gender` ในตัวแปร `user` ให้เป็น factor
- ใช้คำสั่ง `group_by(gender)` เพื่อทำการจัดกลุ่มข้อมูลตามค่าเพศ
- ใช้คำสั่ง `summarise(count = n())` เพื่อนับจำนวนผู้ใช้งานตามกลุ่มเพศ
- และใช้คำสั่ง `mutate(pct = count / sum(count) * 100)` เพื่อทำการหาอัตราส่วนของเพศต่างๆในข้อมูลผู้ใช้งานชุดนี้

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# store a result from data transformation in the variable `gender_summary`
gender_summary <- user %>%
  ... %>%
  ... %>%
  ... %>%
  ...

# print out `gender_summary`
gender_summary
```

*** =solution
```{r}
# store a result from data transformation in the variable `gender_summary`
gender_summary <- user %>%
  mutate(gender = factor(gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))) %>%
  group_by(gender) %>%
  summarise(count = n()) %>%
  mutate(pct = count / sum(count) * 100)

# print out `gender_summary`
gender_summary
```

*** =sct
```{r}
success_msg("Good! You will see that we have arranged the data form so that we can properly use it in data visualization step")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:1ea8a24558
## ทำความรู้จักกับ ggplot2!

การทำ data visualization หรือการแสดงข้อมูลในรูปแบบภาพ อาจจะช่วยให้เราเห็นภาพรวมของข้อมูลเพิ่มขึ้นไม่มากก็น้อย ซึ่งการทำ data visualization เบื้องต้นก็สามารถทำได้หลากหลายรูป ขึ้นอยู่กับรูปแบบของข้อมูล

แม้ว่า R จะมี function สำเร็จรูปบางส่วนที่ช่วยให้เราทำ data visualization ได้ แต่เราก็ยังมี package `ggplot2` ซึ่งช่วยให้การทำ data visualization หลากหลายรูปแบบเป็นเรื่องง่ายขึ้น ไม่ว่าจะเป็น `Bar Chart`, `Histogram`, `Boxplot`, `Scatter Plot` และอื่นๆอีกมากมาย

*** =instructions
ให้คุณพิมพ์คำสั่ง `library("ggplot2")` ลงไปใน editor แล้วกด `submit` เพื่อทำการนำเข้า function ต่างๆจาก package `ggplot2`

*** =hint

*** =pre_exercise_code
```{r}
```

*** =sample_code
```{r}
# import pacakge `ggplot2` here

```

*** =solution
```{r}
# import pacakge `ggplot2` here
library("ggplot2")
```

*** =sct
```{r}
success_msg("Great! Now, you are ready to start learning more about ggplot2!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:2e6bc63534
## Bar Chart (1)

การทำ data visualization รูปแบบแรกที่เราจะพูดถึงกันก็คือแผนภูมิแบบแท่ง (Bar Chart)

ในการใช้งาน package `ggplot2` สิ่งที่คุณต้องทำก็คือการสร้างสิ่งที่เรียกว่า `layer` ซ้อนทับไปเรื่อยๆจนก่อเกิดเป็นแผนภูมิขึ้นมา โดย `layer` แรกสุดก็คือ function `ggplot()` ซึ่งจะเป็นการบอก R ว่าคุณจะใช้ข้อมูลอะไรในการสร้างแผนภูมิ รวมไปถึงการกำหนดแกน `x` และ `y` ต่างๆเบื้องต้นด้วย

คำสั่ง `ggplot(data = dataset, mapping = aes(x = x_coordinate))` เป็นการบอกให้ R สร้าง `layer` แรกสุดจากข้อมูล `dataset` โดยใช้คอลัมน์ `x_coordinate` เป็นแกน `x`

*** =instructions
ให้คุณลองใช้ function `ggplot()` ในการสร้าง `layer` สำหรับแผนภูมิโดยใช้ข้อมูลจาก `user` และกำหนดให้คอลัมน์ `gender` เป็นแกน `x`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# create the first layer with `ggplot()` function here
ggplot(data = ..., mapping = aes(x = ...))
```

*** =solution
```{r}
# create the first layer with `ggplot()` function here
ggplot(data = user, mapping = aes(x = gender))
```

*** =sct
```{r}
success_msg("Good Job! However, you can see from the visualixation that the graph is still empty. This is because you haven't tell R to create the graphic layer yet. Let's go to the next exercise to see how to do it.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:064426575a
## Bar Chart (2)

เมื่อคุณได้สร้าง `layer` แรกเป็นที่เรียบร้อยแล้ว คุณต้องใช้ function `geom_bar()` เพื่อบอกให้ R สร้าง `layer` ต่อไปสำหรับแสดงภาพแผนภูมิแบบแท่ง โดยคุณสามารถทำการซ้อนทับ `layer` ดังกล่าวลงไปบน `layer` `ggplot` ได้เลยโดยใช้เครื่องหมาย `+` ยกตัวอย่างเช่น:

คำสั่ง `ggplot(data = dataset, mapping = aes(x = x_coordinate)) + geom_bar()` จะบอกให้ R ทำการสร้าง `layer` แรกจาก function `ggplot()` ตามด้วยการซ้อน `layer` ที่สร้างจาก function `geom_bar()` เพื่อแสดงภาพแผนภูมิแท่งลงไป

*** =instructions
พิมพ์ `+ geom_bar()` ต่อท้ายคำสั่ง `ggplot()` เดิมใน editor เพื่อทำการสร้างแผนภูมิแท่ง

*** =hint

*** =pre_exercise_code
```{r}
library("ggplot2")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# overlay another layer `geom_bar()` to the back of `ggplot()` function
ggplot(data = user, mapping = aes(x = gender))
```

*** =solution
```{r}
# overlay another layer `geom_bar()` to the back of `ggplot()` function
ggplot(data = user, mapping = aes(x = gender)) + geom_bar()
```

*** =sct
```{r}
success_msg("Yureka! You can see that the visualization was built using a simple `geom_bar` layer but this is not all we can do.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:4b82ef22be
## Bar Chart (3)

อย่างที่คุณเห็นในแบบฝึกหัดที่แล้ว ถ้าข้อมูลที่คุณใส่ลงไปใน function `ggplot()` ไม่ได้มีการจัดรูปแบบให้เหมาะสม R จะทำการสร้างแผนภูมิโดยให้แกน `y` แทนจำนวนนับของข้อมูลใน dataset ที่มีค่าตามแกน `x`

คุณสามารถเปลี่ยนแปลงค่าเหล่านั้นได้โดยการระบุให้แกน `y` มีค่าเป็นคอลัมน์ต่างๆใน dataset เพื่อให้ R แสดงค่าออกมาตามนั้นได้ เพียงแต่ข้อมูลที่คุณใส่ลงไปควรจะถูกจัดให้อยู่ในรูปแบบที่เหมาะสม

ดังนั้น คราวนี้เราจะเปลี่ยนข้อมูลเป็น `gender_summary` ซึ่งถูกเตรียมไว้จากแบบฝึกหัดก่อนๆหน้านี้แทน เพื่อให้การสร้างแผนภูมิแท่งสามารถสื่อความหมายได้ดีขึ้น

*** =instructions
- เปลี่ยน argument `data` ให้มีค่าเป็นตัวแปร `gender_summary` แทนที่จะเป็น `user`
- นอกจากการกำหนดแกน `x = gender` แล้ว ให้คุณเพิ่ม `y = count` ลงไปด้วยเพื่อให้ R แสดงค่าแกน `y` ตามข้อมูลในคอลัมน์ `count`
- เพิ่ม `layer` `geom_bar()` ไว้ด้านหลัง `ggplot()` โดยให้คุณระบุ argument `stat = "identity"` ลงไปภายใน function `geom_bar()` ด้วย เพื่อเป็นการบอก R ให้แสดงค่าแกน `y` ตามค่าที่กำหนดไว้

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")

gender_summary <- user %>%
  mutate(gender = factor(gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))) %>%
  group_by(gender) %>%
  summarise(count = n()) %>%
  mutate(pct = count / sum(count) * 100)
```

*** =sample_code
```{r}
# modify some details to make a bar chart better
ggplot(data = user, mapping = aes(x = gender, ...)) + ...
```

*** =solution
```{r}
# modify some details to make a bar chart better
ggplot(data = gender_summary, mapping = aes(x = gender, y = count)) + geom_bar(stat = "identity")
```

*** =sct
```{r}
success_msg("Good! The graph is now a bit more informative and you can modify the function even more!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:846659ff03
## Bar Chart (4)

แผนภูมิแท่งสีขาวดำอาจทำให้การทำ data visualization ดูไม่สวยงามเท่าไรนัก ดังนั้นเราจะมาเปลี่ยนค่าสีของแผนภูมิกัน

นอกจากคุณจะสามารถกำหนดแกน `x` และ `y` ผ่าน function `aes()` ใน argument `mapping` ได้แล้ว คุณยังสามารถเปลี่ยนสีของ `Bar Chart` ให้แตกต่างกันตามแต่ละกลุ่มข้อมูลได้โดยการเพิ่ม argument `fill` ลงไปใน function `aes()`

*** =instructions
- ให้คุณเปลี่ยนค่าแกน `y` จากการใช้คอลัมน์ `count` เป็น `pct` แทน เพื่อแสดงจำนวนสัดส่วนของผู้ใช้งานเว็บไซต์แต่ละเพศ
- เพิ่ม argument `fill = gender` ลงไปใน function `aes()` เพื่อบอกให้ R เปลี่ยนสี `Bar Chart` ตามค่าในคอลัมน `gender`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")

gender_summary <- user %>%
  mutate(gender = factor(gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))) %>%
  group_by(gender) %>%
  summarise(count = n()) %>%
  mutate(pct = count / sum(count) * 100)
```

*** =sample_code
```{r}
# now, put `fill` argument to adjust the color of the bars to the values of `x` axis
ggplot(data = gender_summary, mapping = aes(x = gender, y = count, ...)) + geom_bar(stat = "identity")
```

*** =solution
```{r}
# now, put `fill` argument to adjust the color of the bars to the values of `x` axis
ggplot(data = gender_summary, mapping = aes(x = gender, y = pct, fill = gender)) + geom_bar(stat = "identity")
```

*** =sct
```{r}
success_msg("You're doing good! And what if we just want to change the bar color to something other than black and resize the bars? Let's see the next exercise!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:ad4e0721cc
## Bar Chart (5)

** contents regarding `fill` with a single color and `width` to control the bar width

*** =instructions
- ลบ argument `fill = gender` ออกจาก function `aes()` เพื่อยกเลิกการกำหนดสีตามกลุ่มเพศ
- เพิ่ม argument `fill = "#48b6a3"` และ `width = 0.5` ลงไปใน function `geom_bar()` แทนเพื่อเปลี่ยนสีและขนาด `Bar Chart`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")

gender_summary <- user %>%
  mutate(gender = factor(gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))) %>%
  group_by(gender) %>%
  summarise(count = n()) %>%
  mutate(pct = count / sum(count) * 100)
```

*** =sample_code
```{r}
# switch `fill` to `geom_bar()` function instead and also put more arguments there
ggplot(data = gender_summary, mapping = aes(x = gender, y = pct, fill = gender)) + 
  geom_bar(stat = "identity", ..., ...)
```

*** =solution
```{r}
# switch `fill` to `geom_bar()` function instead and also put more arguments there
ggplot(data = gender_summary, mapping = aes(x = gender, y = pct)) + 
  geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5)
```

*** =sct
```{r}
success_msg("Good Job! But looks like something's still missing... Would it be better if we also name the chart and give its axis a meaningful name?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:fe1746b0ca
## Bar Chart (6)

** contents regarding naming the graph & axis using `labs` with arguments `title`, `x`, and `y`

*** =instructions
เพิ่มอีก `layer` หนึ่งด้วย function `labs()` ตั้งชื่อให้กับแผนภูมิ แกน `x` และแกน `y` ว่า `"Wongnai's Users: Gender Distribution"`, `"Gender"` และ `"Proportion (%)"` ตามลำดับ อย่าลืมใส่ double quotes เพื่อบอก R ว่าข้อมูลเหล่านี้เป็นข้อมูลตัวอักษรด้วย

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")

gender_summary <- user %>%
  mutate(gender = factor(gender, levels=c(1, 2, 0), labels=c('male', 'female', 'unknown'))) %>%
  group_by(gender) %>%
  summarise(count = n()) %>%
  mutate(pct = count / sum(count) * 100)
```

*** =sample_code
```{r}
# switch `fill` to `geom_bar()` function instead and also put more arguments there
ggplot(data = gender_summary, mapping = aes(x = gender, y = pct)) + 
  geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
  labs(title = ..., x = ..., y = ...)
```

*** =solution
```{r}
# switch `fill` to `geom_bar()` function instead and also put more arguments there
ggplot(data = gender_summary, mapping = aes(x = gender, y = pct)) + 
  geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
  labs(title="Wongnai's Users: Gender Distribution", x="Gender", y="Proportion (%)")
```

*** =sct
```{r}
success_msg("Very good!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:7362d84d34
## ร้านอาหารที่มีจำนวนสาขาเยอะที่สุด! (1)

คราวนี้เราจะลองเปลี่ยนมาสร้าง Bar Chart จากข้อมูลร้านอาหารกันบ้าง แต่ก่อนที่จะสร้าง Bar Chart ได้ คุณจะต้องจัดการกับข้อมูลก่อน!

เราจะใช้ package `dplyr` เพื่อทำการหาร้านอาหารที่มีจำนวนสาขามากที่สุด 10 อันดับแรกกัน!

สำหรับข้อมูลร้านอาหารที่เรามีใน data frame `restaurant` นั้น ข้อมูล 1 แถวจะเป็นตัวแทนของร้านอาหาร 1 ร้าน การที่จะบอกว่าร้านอาหารใดอยู่ภายใต้แบรนด์เดียวกันเราจึงต้องอาศัย `chain_id` เข้ามาช่วย เช่น ร้านที่อยู่ภายใต้แบรนด์ `Starbucks` ก็จะมี `chain_id` เป็น 1 เหมือนกันทุกร้าน เป็นต้น

*** =instructions
ให้คุณทำการสรุปจำนวนสาขาของร้านอาหารแต่ละร้านโดยใช้ pipes (`%>%`) และปฏิบัติตามขั้นตอนต่อไปนี้:

- ใช้ function `filter()` คัดกรองข้อมูลจาก data frame `restaurant` โดยให้คอลัมน์ `chain_id != 0` เพื่อตัดร้านอาหารที่เป็นร้านเฉพาะ ไม่มีสาขาย่อยออกไป
- ใช้ function `group_by()` เพื่อจัดกลุ่มข้อมูลตามคอลัมน์ `chain_id`
- ใช้ function `summarise()` เพื่อสรุปข้อมูลหลังจากที่ได้ทำการจัดกลุ่มแล้ว โดยการนับจำนวนร้านอาหารหรือ `count = n()` ตามกลุ่ม `chain_id`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# `filter()`, `group_by()` and then `summarise()` the `restaurant` data frame
restaurant %>% 
  ... %>% 
  ... %>% 
  ...
```

*** =solution
```{r}
# `filter()`, `group_by()` and `summarise()` the `restaurant` data frame
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n())
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:1e67f78696
## ร้านอาหารที่มีจำนวนสาขาเยอะที่สุด! (2)

ตามปกติแล้ว การจัดเรียงข้อมูลและตัดข้อมูลเฉพาะ 10 อันดับแรกมักจะต้องใช้ function แยกกันเพื่อทำเป็นขั้นๆ เช่น เริ่มเรียงข้อมูลด้วย `arrange()` และตัดข้อมูลด้วย `head()` แต่ package `dplyr` มีอีกหนึ่งตัวช่วยที่ช่วยให้เราสามารถเรียงข้อมูลและตัดข้อมูลได้ในคำสั่งเดียว คำสั่งนั้นก็คือ `top_n()`

คำสั่ง `top_n(x, y)` จะทำการจัดเรียงข้อมูลตามคอลัมน์ `y` และตัดข้อมูลออกมา `x` แถว ซึ่งตามปกติแล้ว function นี้จะเรียงจากมากไปหาน้อย แต่หากคุณต้องให้เรียงจากน้อยไปหามากก็สามารถทำได้ด้วยการใส่เครื่องหมาย `-` ไว้ข้างหน้า `x`

*** =instructions
- ใช้ function `top_n()` โดยทำการเรียงลำดับตามจำนวนร้านอาหารที่อยู่ภายใต้ `chain_id` นั้นๆ และตัดข้อมูลออกมาแสดงแค่ `10` อันดับแรก
- นำข้อมูลนี้ไป `inner_join()` กับ data frame `chain` ด้วยคอลัมน์ `chain_id` และ `id` เพื่อดึงรายละเอียดของ `chain_id` นั้นๆออกมา

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# use `top_n()` and `inner_join()` function with the `chain` data frame to finish the code
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  ... %>%
  ...
```

*** =solution
```{r}
# use `top_n()` and `inner_join()` function with the `chain` data frame to finish the code
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  top_n(10, count) %>%
  inner_join(chain, by = c("chain_id" = "id"))
```

*** =sct
```{r}
success_msg("Good!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:5e87713a93
## More on Bar Chart (1)

หลังจากที่ได้ทำการวิเคราะห์ข้อมูลโดยใช้ package `dplyr` ไปแล้ว เราจะมาเริ่มทำ data visualization กัน!

ถ้าคุณยังจำได้ จากบทที่แล้ว นอกจากเราจะสามารถใช้ pipes (`%>%`) ในการจัดการข้อมูลเป็นลำดับขั้นในคำสั่งเดียวแล้ว เรายังสามารถนำ pipes (`%>%`) มาใช้กับการทำ data visualization ได้ด้วยเช่นกัน

*** =instructions
ต่อจากแบบฝึกหัดที่แล้ว ให้คุณใช้ pipes (`%>%`) ร่วมกับ package `ggplot2` ในการสร้าง Bar Chart

- สร้าง `layer` แรกด้วยคำสั่ง `ggplot(aes(x = factor(name, levels = name), y = count))` เพื่อบอกให้ R ใช้คอลัมน์ `name` แบบ factor เป็นแกน `x` 
- สร้าง `layer` ที่สองด้วยคำสั่ง `geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5)` เพื่อเปลี่ยนสีแท่งข้อมูลเป็นสีฟ้าและให้ความกว้างของแท่งข้อมูลเท่ากับ `0.5`
- สร้าง `layer` ที่สามด้วยคำสั่ง `labs(x = "", y = "Count", title = "Top Chain Restaurants by Number of Branches")` เพื่อกำหนดชื่อให้แผนภูมิและแกนต่างๆ

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# create `layers` using `ggplot()`, `geom_bar()`, and `labs()` to create a bar chart!
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  top_n(10, count) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  ... +
    ... +
    ...
```

*** =solution
```{r}
# create `layers` using `ggplot()`, `geom_bar()`, and `labs()` to create a bar chart!
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  top_n(10, count) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  ggplot(aes(x = factor(name, levels = name[order(count)]), y = count)) +
    geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
    labs(x = "", y = "Count", title = "Top Chain Restaurants by Number of Branches")
```

*** =sct
```{r}
success_msg("Good!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:2693650430
## More on Bar Chart (2)

คุณอาจสามารถทำให้แผนภูมิเป็นระเบียบขึ้นได้โดยการเรียงลำดับข้อมูล

*** =instructions
เปลี่ยน argument `levels = name` ใน function `aes()` เป็น `levels = name[order(count)]` เพื่อเรียงลำดับข้อมูลในแกน `x` ตามค่าในคอลัมน์ `count`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# sort the factor by change the argument `levels = name` to `levels = name[order(count)]`
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  top_n(10, count) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  ggplot(aes(x = factor(name, levels = name), y = count)) +
    geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
    labs(x = "", y = "Count", title = "Top Chain Restaurants by Number of Branches")
```

*** =solution
```{r}
# sort the factor by change the argument `levels = name` to `levels = name[order(count)]`
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  top_n(10, count) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  ggplot(aes(x = factor(name, levels = name[order(count)]), y = count)) +
    geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
    labs(x = "", y = "Count", title = "Top Chain Restaurants by Number of Branches")
```

*** =sct
```{r}
success_msg("Good Job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c425fce1b4
## More on Bar Chart (3)

นอกจากการเรียงลำดับข้อมูลแล้ว คุณยังสามารถทำการสลับแกน `x` และ `y` ได้โดยการเพิ่ม `layer` `coord_flip()` ซึ่งการสลับแกนข้อมูลอาจจะเป็นประโยชน์ในการวิเคราะห์ข้อมูลบางครั้ง

*** =instructions
เพิ่ม `layer` `coord_flip()` ลงไปด้านหลัง function `labs()` และกด `submit` เพื่อดูผล

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# add `coord_flip()` to the end of `labs()` function
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  top_n(10, count) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  ggplot(aes(x = factor(name, levels = name[order(count)]), y = count)) +
    geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
    labs(x = "", y = "Count", title = "Top Chain Restaurants by Number of Branches") +
    ...
```

*** =solution
```{r}
# add `coord_flip()` to the end of `labs()` function
restaurant %>% 
  filter(chain_id != 0) %>% 
  group_by(chain_id) %>% 
  summarise(count = n()) %>%
  top_n(10, count) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  ggplot(aes(x = factor(name, levels = name[order(count)]), y = count)) +
    geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
    labs(x = "", y = "Count", title = "Top Chain Restaurants by Number of Branches") +
    coord_flip()
```

*** =sct
```{r}
success_msg("Cool!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:45a27018ad
## การวิเคราะห์ข้อมูลตัวแปรเชิงปริมาณ (1)

ในแบบฝึกหัดก่อนๆหน้านี้ เราทำการวิเคราะห์เพียงแต่ตัวแปรเชิงคุณภาพ (`qualitative variable`) หรือตัวแปรแบบแบ่งกลุ่ม (`categorical variable`) มาโดยตลอด สิ่งที่เราได้ทำมีเพียงแค่การนับว่าแต่ละกลุ่มมีข้อมูลจำนวนเท่าไรเท่านั้น

ตั้งแต่แบบฝึกหัดนี้เราจะเปลี่ยนมาวิเคราะห์ตัวแปรเชิงปริมาณ (`quantitative variable`) หรือตัวแปรแบบต่อเนื่อง (`continuous variable`) กันบ้าง ผ่าน data frame `restaurant`, `restaurant_checkin_user` และ `chain`

*** =instructions
- ใช้ function `filter()` เพื่อกรองเฉพาะร้านอาหารที่เป็นร้านสาขา (`chain_id != 0`) และเป็นร้านอาหารจริงๆ (`domain_id == 1`)
- ใช้ function `select()` เลือกคอลัมน์ `id` และ `chain_id` เพื่อตัดคอลัมน์ที่ไม่จำเป็นออกไป
- เก็บค่าผลลัพธ์ไว้ในตัวแปร `chain_checkin_summary`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")
```

*** =sample_code
```{r}
# complete the code using `filter()` and `select()`
... <- restaurant %>%
  filter(
    ..., 
    ...
  ) %>%
  select(..., ...)
```

*** =solution
```{r}
# complete the code using `filter()` and `select()`
chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:7ef0e8802b
## การวิเคราะห์ข้อมูลตัวแปรเชิงปริมาณ (2)

เราจะทำการคำนวณจำนวนร้านอาหารในแต่ละแบรนด์ (แบ่งกลุ่มตาม `chain_id`) รวมถึงจำนวน `check_in` ที่เกิดขึ้นกัน

เพื่อไม่ให้ข้อมูลร้านอาหารที่ไม่เคยมีการ `check_in` หายไป เราจะต้องใช้ function `left_join()` เพื่อเชื่อมข้อมูลจาก `restaurant` ในแบบฝึกหัดที่แล้วไปยัง `restaurant_checkin_user`

*** =instructions
- ใช้ function `left_join()` เพื่อเชื่อม คอลัมน์ `id` ของ `restaurant` เข้ากับ `restaurant_id` ของ `restaurant_checkin_user` อย่าลืมว่าคุณต้องใส่ double quotes (`"`) ให้ชื่อคอลัมน์ด้วย
- ใช้ function `group_by()` เพื่อจัดกลุ่มร้านอาหารตาม `chain_id`
- ใช้ function `summarise()` เพื่อนับจำนวนร้านอาหารแบบไม่ซ้ำร้าน (`n_distinct(id)`) และนับจำนวนเช็คอิน (`sum(check_in)`) ซึ่งถ้ามีการเช็คอิน คอลัมน์ `check_in` จะมีค่าเป็น 1 แต่ถ้าไม่มีจะมีค่าเป็น 0 ตั้งชื่อให้คอลัมน์ใหม่ว่า `n_restaurants` และ `n_checkins` ตามลำดับ

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")
```

*** =sample_code
```{r}
# complete the code using `left_join()`, `group_by()` and `summarise()`
chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(..., by = c(... = ...)) %>%
  group_by(...) %>% 
  summarise(
    n_restaurants = ...,
    n_checkins = ...
  )
```

*** =solution
```{r}
# complete the code using `left_join()`, `group_by()` and `summarise()`
chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  )
```

*** =sct
```{r}
success_msg("Good!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:1c65707c3c
## การวิเคราะห์ข้อมูลตัวแปรเชิงปริมาณ (3)

ต่อจากแบบฝึกหัดที่แล้ว คราวนี้เราจะนำผลลัพธ์ที่สร้างไว้มาเชื่อมกับ data frame `chain` และทำการดึงข้อมูลชื่อแบรนด์ร้านอาหารออกมาเพื่อทำให้ข้อมูลของเราสมบูรณ์มากขึ้น

*** =instructions
- ใช้ function `inner_join()` เพื่อเชื่อมคอลัมน์ `chain_id` ของผลลัพธ์จากแบบฝึกหัดที่แล้วกับคอลัมน์ `id` ของตัวแปร `chain` อย่าลืมว่าคุณต้องใส่ double quotes (`"`) ด้วย!
- ใช้ function `select()` เพื่อดึงคอลัมน์ `chain_id`, `n_restaurants`, `n_checkins` และ `name`
- ใช้ function `mutate()` เพื่อเปลี่ยนแปลงค่า `NA` ในคอลัมน์ `n_checkins` ให้เป็น `0` (เนื่องจากการใช้ `left_join()` ร้านอาหารที่ไม่เคยได้รับการเช็คอินเลยจะมีค่าในคอลัมน์ `n_checkins` เป็น `NA`) คุณสามารถใส่คำสั่ง `n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)` ลงไปใน function `mutate()` ได้เลย
- ใช้ function `arrange()` เพื่อเรียงลำดับข้อมูลที่เราจัดไว้ โดยเรียงลำดับตามคอลัมน์ `n_checkins` โดยเรียงจากมากไปหาน้อย (คุณสามารถใช้เครื่องหมาย `-` หรือ function `desc()` ก็ได้)
- สั่งให้ R แสดงค่าข้อมูล 10 แถวแรกจาก `chain_checkin_summary`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")
```

*** =sample_code
```{r}
# complete the code using `inner_join()`, `select()` and `arrange()`
chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  ... %>%
  ... %>%
  ... %>%
  ...
  
# display the first 10 rows from `chain_checkin_summary` here

```

*** =solution
```{r}
# complete the code using `inner_join()`, `select()` and `arrange()`
chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
  
# display the first 10 rows from `chain_checkin_summary` here
head(chain_checkin_summary, n = 10)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:2ba2347975
## การวิเคราะห์ข้อมูลตัวแปรเชิงปริมาณ (4)

หลังจากที่เราได้ข้อมูลที่ต้องการแล้ว เราจะมาเริ่มทำการวิเคราะห์กัน!

ปกติแล้ว ในการวิเคราะห์ข้อมูลเชิงปริมาณ เรามักจะสนใจการกระจายตัวของข้อมูล ซึ่งสิ่งที่จะบอกถึงการกระจายตัวของข้อมูลเบื้องต้นได้ก็คือค่าที่เรียกว่า `5-number summary` ซึ่งประกอบไปด้วย `Min`, `1st Quantile`, `Median`, `3rd Quantile` และ `Max`

ค่าเหล่านี้จะครอบคลุมจุดต่างๆที่อยู่ในข้อมูลเชิงปริมาณแต่ละชุด

*** =instructions
ใน workspace มีตัวแปร `chain_checkin_summary` จากแบบฝึกหัดที่แล้วอยู่ ให้คุณลองทำความเข้าใจกับ code ที่อยู่ใน editor จากนั้นกด `submit` เพื่อดูว่า function `fivenum()` และ `quantile()` สามารถช่วยเราในการวิเคราะห์ข้อมูลเชิงปริมาณได้อย่างไร

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# hit `submit` when you're ready!
fivenum(chain_checkin_summary$n_restaurants)

quantile(chain_checkin_summary$n_checkins)
```

*** =solution
```{r}
# hit `submit` when you're ready!
fivenum(chain_checkin_summary$n_restaurants)

quantile(chain_checkin_summary$n_checkins)
```

*** =sct
```{r}
success_msg("Good Job! Now, you get a very rough overall picture of how your continuous data are")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:fd3e291690
## การวิเคราะห์ข้อมูลตัวแปรเชิงปริมาณ (5)

มีหลายวิธีที่ทำให้เราสามารถคำนวณตัวเลข `5-number summary` ได้ และแต่ละตัวก็มี function เป็นของตัวเองในกรณีที่คุณอยากจะคำนวณค่าเหล่านั้นมาใช้ function เหล่านี้ ได้แก่

- `min(x)` สำหรับคำนวณค่าที่น้อยที่สุดในข้อมูล `x`
- `median(x)` สำหรับคำนวณค่าที่อยู่กึ่งกลางของข้อมูล `x`
- `max(x)` สำหรับคำนวณค่าที่มากที่สุดในข้อมูล `x`
- `quantile(x, y)` สำหรับคำนวณค่าข้อมูล ณ percentile ที่ `y` ของข้อมูล `x` ในกรณีที่ไม่มีการระบุตำแหน่ง percentile ที่ต้องการ `quantile(x)` จะทำการแสดงค่า `5-number summary` ของ `x` ดังที่ได้เห็นไปในแบบฝึกหัดที่แล้ว

*** =instructions
ให้คุณทำตามคำสั่งที่ปรากฏใน editor โดยพิมพ์ code ให้ถูกต้องเพื่อคำนวณค่าต่างๆจากตัวแปร `chain_checkin_summary`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# calculate minimum value from `n_checkins` column


# calculate 25th percentile value from `n_checkins` column


# calculate median value from `n_checkins` column


# calculate 75th percentile value from `n_checkins` column


# calculate maximum value from `n_checkins` column

```

*** =solution
```{r}
# calculate minimum value from `n_checkins` column
min(chain_checkin_summary$n_checkins)

# calculate 25th percentile value from `n_checkins` column
quantile(chain_checkin_summary$n_checkins, 0.25)

# calculate median value from `n_checkins` column
median(chain_checkin_summary$n_checkins)

# calculate 75th percentile value from `n_checkins` column
quantile(chain_checkin_summary$n_checkins, 0.75)

# calculate maximum value from `n_checkins` column
max(chain_checkin_summary$n_checkins)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:2d8ac6ddaf
## การวิเคราะห์ข้อมูลตัวแปรเชิงปริมาณ (6)

ในการคำนวณ `5-number summary` อาจมีวิธีง่ายกว่าการใช้ function `fivenum()` หรือ `quantile()` แถมยังสามารถใช้สรุปข้อมูลทุกคอลัมน์ใน data frame ได้ในคำสั่งเดียวทั้งตัวแปรเชิงคุณภาพและปริมาณ

คำสั่งนั้นก็คือ `summary()` ที่ทุกคนคุ้นเคย

*** =instructions
ใช้ function `summary()` กับ `chain_checkin_summary` เพื่อแสดงสรุปลักษณะข้อมูลทุกคอลัมน์

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}

```

*** =solution
```{r}
summary(chain_checkin_summary)
```

*** =sct
```{r}
success_msg("Cool! Look at the summary result. Do you notice something in our data distribution?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:d22160bf3e
## ทำความรู้จักกับ Outlier

จากแบบฝึกหัดที่แล้ว คุณจะเห็นได้ว่าข้อมูลใน `chain_checkin_summary` มีความเบ้ซ้ายค่อนข้างสูง นั่นคือข้อมูลแบรนด์ร้านอาหารส่วนใหญ่มีจำนวนร้านอาหารและจำนวนการเช็คอินค่อนข้างกระจุกตัวอยู่ที่ต่ำกว่า 5 ร้านต่อแบรนด์และต่ำกว่า 12 เช็คอินต่อแบรนด์ตามลำดับ ซึ่งเมื่อเกิดปัญหาแบบนี้ขึ้น การใช้ `median()` หรือค่ามัธยฐานเป็นตัวแทนของข้อมูลจะค่อนข้างให้ผลดีกว่าการใช้ `mean()` หรือค่าเฉลี่ย เพราะค่า `mean()` นั้นจะได้รับผลกระทบจาก `outlier` ในข้อมูลค่อนข้างมากหากข้อมูลมีจำนวนไม่มากพอ

`outlier` คือค่าผิดปกติที่มีค่าแตกต่างจากข้อมูลที่เหลือใน dataset มากอย่างเห็นได้ชัด กรณีตัวอย่างเช่น หากเรามีข้อมูลความสูงของมนุษย์ แล้วพบว่าในข้อมูลมีมนุษย์ที่มีความสูง 2.5 เมตรหรือน้อยกว่า 1 เมตร ไม่ต้องบอกเราก็สามารถรู้ได้ทันทีว่าข้อมูลนี้เป็นข้อมูลที่ผิดปกติ

เพื่อให้เห็นภาพมากขึ้น เราจะมาลองดูผลใน editor กัน

*** =instructions
กด submit เพื่อดูความแตกต่างของการใช้ `median()` และ `mean()`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# click `submit` to see the result and inspect difference in `median()` and `mean()` value
paste("Median of n_restaurants is", median(chain_checkin_summary$n_restaurants))
paste("Mean of n_restaurants is", mean(chain_checkin_summary$n_restaurants))

paste("Median of n_checkins is", median(chain_checkin_summary$n_checkins))
paste("Mean of n_checkins is", mean(chain_checkin_summary$n_checkins))
```

*** =solution
```{r}
# click `submit` to see the result and inspect difference in `median()` and `mean()` value
paste("Median of n_restaurants is", median(chain_checkin_summary$n_restaurants))
paste("Mean of n_restaurants is", mean(chain_checkin_summary$n_restaurants))

paste("Median of n_checkins is", median(chain_checkin_summary$n_checkins))
paste("Mean of n_checkins is", mean(chain_checkin_summary$n_checkins))
```

*** =sct
```{r}
success_msg("Now, you will see that values of medians are quite different from those of means. So, how can we solve the problem of having outliers in our dataset?")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:acccc43ea5
## Truncated Mean

วิธีหนึ่งที่สามารถใช้ได้ในการแก้ไขปัญหา outlier ในข้อมูลคือการใช้ `Truncated Mean`

`Truncated Mean` คือการหาค่าเฉลี่ยโดยการตัดปลายหางของข้อมูลบางส่วนทิ้งไป ซึ่งเราสามรถกำหนดระยะของข้อมูลที่เราต้องการตัดทิ้งได้ด้วยค่า percentile

ปกติแล้ว `outlier` จะเป็นค่าที่น้อยมากหรือสูงมากจนผิดปกติ นั่นทำให้มันมักจะอยู่บริเวณปลายสุดของข้อมูลทั้งสองข้าง การใช้ `Truncated Mean` จึงเป็นตัวเลือกหนึ่งที่ช่วยให้เราสามารถแก้ปัญหาในการหาค่าเฉลี่ยเพื่อเป็นตัวแทนข้อมูลที่เหมาะสมมากขึ้นได้

คำสั่ง `mean(x, trim = 0.1)` จะทำการตัดข้อมูลออกข้างละ `5%` ทั้งที่ต่ำที่สุดและสูงที่สุดออกจากข้อมูลก่อนนำมาคำนวณค่าเฉลี่ย

*** =instructions
ให้คุณทำการหาค่า `mean` และ `truncated mean` โดยการตัดข้อมูลออกข้างละ `0.5%` และ `2.5%` ตามลำดับ อย่าลืมว่าคุณต้องใช้เพิ่ม argument `trim` เพื่อทำการคำนวณ `truncated mean`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# Finding each means on `n_restaurants` column
mean(chain_checkin_summary$n_restaurants)
mean(chain_checkin_summary$n_restaurants, ...)
mean(chain_checkin_summary$n_restaurants, ...)

# Finding each means on `n_checkins` column
mean(chain_checkin_summary$n_checkins)
mean(chain_checkin_summary$n_checkins, ...)
mean(chain_checkin_summary$n_checkins, ...)
```

*** =solution
```{r}
# Finding each means on `n_restaurants` column
mean(chain_checkin_summary$n_restaurants)
mean(chain_checkin_summary$n_restaurants, trim = 0.01)
mean(chain_checkin_summary$n_restaurants, trim = 0.05)

# Finding each means on `n_checkins` column
mean(chain_checkin_summary$n_checkins)
mean(chain_checkin_summary$n_checkins, trim = 0.01)
mean(chain_checkin_summary$n_checkins, trim = 0.05)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:5ac7d8a096
## Winsorized Mean (1)

นอกจากการใช้ `Truncated Mean` แล้ว `Winsorized Mean` หรือ `Censored Mean` ก็เป็นอีกวิธีหนึ่งในการจัดการกับ `outlier` ที่ดีเช่นกัน

ข้อดีของการใช้ `Winsorized Mean` คือ เราจะไม่มีการตัดข้อมูลทิ้ง แต่จะทำการแทนที่ `outlier` ด้วยค่า ณ จุด percentile สุดท้ายที่เราจะยอมรับว่าค่านี้ไม่ใช่ `outlier` แทน เช่น ถ้าเราคิดว่าค่าขอบเขตของข้อมูลที่เหมาะสมควรมีค่าไม่น้อยกว่า percentile ที่ 1 และมีค่าไม่เกิน percentile ที่ 99 เราจะแทนที่ `outlier` ด้วยค่า ณ percentile เหล่านี้ ขึ้นอยู่กับว่า `outlier` ที่เราเจอเป็นฝั่งที่มีค่าน้อยผิดปกติหรือมากผิดปกติ ซึ่งวิธีนี้อาจช่วยลดผลกระทบของ `outlier` ที่มีต่อ `mean` ได้ แต่อาจไม่ได้ช่วยกำจัดผลทิ้งไปทั้งหมด

น่าเสียดายที่ R ไม่มี function สำเร็จรูปสำหรับการหา `Winsorized Mean` ดังนั้นเราจึงต้องทำการจัดการกับ `outlier` ในข้อมูลด้วยตัวเองเสียก่อน แล้วค่อยทำการหาค่า `mean()` ตามปกติ

*** =instructions
ให้คุณคำนวณค่า percentile ที่ 1 และ 99 ของคอลัมน์ `n_restaurants` จาก `chain_checkin_summary` แล้วเก็บผลลัพธ์ไว้ในตัวแปร `pct01` และ `pct99` ตามลำดับ

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# calculate 1st and 99th percentile of `n_restaurants` with `quantile()` function and store result in `pct01`
pct01 <- ...
pct99 <- ...
```

*** =solution
```{r}
# calculate 1st and 99th percentile of `n_restaurants` with `quantile()` function and store result in `pct01`
pct01 <- quantile(chain_checkin_summary$n_restaurants, 0.01)
pct99 <- quantile(chain_checkin_summary$n_restaurants, 0.99)
```

*** =sct
```{r}
success_msg("Good Job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:48798787ff
## Winsorized Mean (2)

ลำดับต่อมา คุณจะต้องทำการเปลี่ยนแปลงรูปแบบข้อมูลใน `chain_checkin_summary` ก่อนแล้วจึงทำการหา `mean()` ตามปกติ

ได้เวลาดึงความรู้ package `dplyr` กลับมาใช้แล้ว!

*** =instructions

- ใช้ function `mutate()` เพื่อทำการเปลี่ยนแปลงข้อมูลในคอลัมน์ `n_restaurants` ในกรณีที่ข้อมูลมีค่าน้อยกว่า `pct01` ให้มีค่าเท่ากับ `pct01` แต่ถ้ามีค่ามากกว่า `pct99` ให้มีค่าเท่ากับ `pct99` ตั้งชื่อคอลัมน์ใหม่ว่า `winsorized_value` (คุณสามารถใช้คำสั่ง `ifelse(n_restaurants < pct01, pct01, n_restaurants)` เพื่อจัดการกับ `outlier` ที่มีค่าน้อยกว่า `pct01` ได้ ให้ทำแบบเดียวกันกับ `outlier` ที่มีค่ามากกว่า `pct99` ด้วย
- คุณสามารถสร้างคอลัมน์ใหม่ซ้ำกันหลายครั้งได้ภายใน function `mutate()` เพื่อเปลี่ยนแปลงค่าในคอลัมน์จนกว่าจะได้ค่าที่คุณต้องการ
- ใช้ function `summarise()` เพื่อทำการสรุปข้อมูล คำนวณค่าเฉลี่ย `winsorized_mean` ด้วยคำสั่ง `mean(winsorized_value)`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
pct01 <- quantile(chain_checkin_summary$n_restaurants, 0.01)
pct99 <- quantile(chain_checkin_summary$n_restaurants, 0.99)

# calculate the `winsorized mean` using `mutate()` and `summarise()` on `chain_checkin_summary`
chain_checkin_summary %>%
  mutate(
    winsorized_value = ...,
    winsorized_value = ...
  ) %>%
  summarise(winsorized_mean = ...)
```

*** =solution
```{r}
pct01 <- quantile(chain_checkin_summary$n_restaurants, 0.01)
pct99 <- quantile(chain_checkin_summary$n_restaurants, 0.99)

# calculate the `winsorized mean` using `mutate()` and `summarise()` on `chain_checkin_summary`
chain_checkin_summary %>%
  mutate(
    winsorized_value = ifelse(n_restaurants < pct01, pct01, n_restaurants),
    winsorized_value = ifelse(n_restaurants > pct99, pct99, n_restaurants)
  ) %>%
  summarise(winsorized_mean = mean(winsorized_value))
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:74f7a005c8
## ความผันผวนและส่วนเบี่ยงเบนมาตรฐาน (1)

ในการวิเคราะห์ข้อมูล อีกปัจจัยหนึ่งที่นักสถิติต้องให้ความสำคัญก็คือการกระจายตัวของข้อมูล ซึ่งเราสามารถวัดขนาดการกระจายตัวของข้อมูลได้โดยการคำนวณค่าความผันผวน (`Variance`) และส่วนเบี่ยงเบนมาตรฐาน (`Standard Deviation` หรือ `SD`)

ค่าทั้งสองจะช่วยให้เราสามารถประมาณค่าการกระจายตัวของข้อมูลโดยใช้ค่าเฉลี่ยเป็นจุดกึ่งกลางได้

แท้จริงแล้ว ส่วนเบี่ยงเบนมาตรฐานก็คือรากที่ 2 ของค่าความผันผวน ซึ่งการพิจารณาส่วนเบี่ยงเบนมาตรฐานจะช่วยให้เราตีความการกระจายตัวของข้อมูลได้ดีกว่าเนื่องจากมันมีหน่วยเดียวกันกับข้อมูลที่เราพิจารณา

*** =instructions

- ให้คุณคำนวณค่าความผันผวน (`Variance`) ของ `n_checkins` โดยใช้ function `var()`
- ให้คุณคำนวณค่าส่วนดบี่ยงเบนมาตรฐาน (`Standard Deviation`) ของ `n_checkins` โดยใช้ function `sd()`
- ให้คุณคำนวณส่วนเบี่ยงเบนมาตรฐานอีกครั้ง แต่คราวนี้ให้ใช้ function `sqrt()` กับ `var()` ของคอลัมน์ `n_checkins` และดูว่ามีค่าเท่ากับในข้อก่อนหน้าหรือไม่

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# (Sampled) Variance
# Mean squared deviation -- how far the observations are spread out from their mean


# (Sampled) Standard Deviation
# Because it has the same unit as the original data, it's usually more interpretable.


# Now, calculate the Standard Deviation again using `sqrt()` on `var()` of `n_checkins`

```

*** =solution
```{r}
# (Sampled) Variance
# Mean squared deviation -- how far the observations are spread out from their mean
var(chain_checkin_summary$n_checkins)

# (Sampled) Standard Deviation
# Because it has the same unit as the original data, it's usually more interpretable.
sd(chain_checkin_summary$n_checkins)

# Now, calculate the Standard Deviation again using `sqrt()` on `var()` of `n_checkins`
sqrt(var(chain_checkin_summary$n_checkins))
```

*** =sct
```{r}
success_msg("Good Job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:d62296e311
## ความผันผวนและส่วนเบี่ยงเบนมาตรฐาน (2)

ปกติแล้ว ถ้าข้อมูลมีการแจกแจงแบบปกติ (`Normal Distribution`) หรือก็คือมีการกระจายตัวความหนาแน่นของข้อมูลรอบๆค่าเฉลี่ยเป็นทรงระฆังคว่ำ เราจะเห็นได้ว่าข้อมูลกว่า `95%` จะกระจุกตัวกันอยู่ภายในบริเวณไม่เกิน 2 `Standard Deviation` นับจากค่าเฉลี่ย

*** =instructions
เราได้ทำการสร้าง code สำหรับคำนวณค่าเฉลี่ยและส่วนเบี่ยงเบนมาตรฐาน (`Standard Deviation`) ของ `n_checkins` ไว้ให้คุณแล้วใน editor ให้คุณทำการคำนวณว่ามีข้อมูลคิดเป็นสัดส่วนเท่าไรที่อยู่ภายในช่วง 2 `Standard Deviation` จากค่าเฉลี่ยของ `n_checkins`
เนื่องจากค่า `True` และ `False` จะมีค่าเป็น `1` และ `0` ตามลำดับ คุณจึงสามารถใช้ function `mean()` ร่วมกับการตรวจสอบเงื่อนไขแบบ `Boolean` เพื่อหาอัตราส่วนของข้อมูลในแบบฝึกหัดนี้ได้

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# calculations of `mean()` and `sd()` of `n_checkins`
mean_checkins <- mean(chain_checkin_summary$n_checkins)
sd_checkins <- sd(chain_checkin_summary$n_checkins)

# Now, it's your turn to calculate the proportion of `n_checkins` data that lies within 2 `sd_checkins` from `mean_checkins`
mean(chain_checkin_summary$n_checkins < ... + 2 * ... &
  chain_checkin_summary$n_checkins > ... - 2 * ...)
```

*** =solution
```{r}
# calculations of `mean()` and `sd()` of `n_checkins`
mean_checkins <- mean(chain_checkin_summary$n_checkins)
sd_checkins <- sd(chain_checkin_summary$n_checkins)

# Now, it's your turn to calculate the proportion of `n_checkins` data that lies within 2 `sd_checkins` from `mean_checkins`
mean(chain_checkin_summary$n_checkins < mean_checkins + 2 * sd_checkins &
  chain_checkin_summary$n_checkins > mean_checkins - 2 * sd_checkins)
```

*** =sct
```{r}
success_msg("Good Job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:76af3280db
## สัมประสิทธิ์ส่วนเบี่ยงเบนมาตรฐาน

สัมประสิทธิ์ส่วนเบี่ยงเบนมาตรฐาน (`Coefficient of Variation` หรือ `CV`) เป็นค่าทางสถิติอีกตัวหนึ่งซึ่งเป็นตัวแทนของค่าการเบี่ยงเบนของข้อมูลต่อ 1 หน่วยค่าเฉลี่ย ซึ่งคุณสามารถคำนวณสัมประสิทธิ์ส่วนเบี่ยงเบนมาตรฐานได้ด้วยการนำส่วนเบี่ยงเบนมาตรฐาน (`Standard Deviation`) มาหารด้วยค่าเฉลี่ย

ข้อจำกัดของสัมประสิทธิ์ส่วนเบี่ยงเบนมาตรฐานคือ ข้อมูลที่จะนำมาพิจารณาต้องมีค่ามากกว่าหรือเท่ากับ `0`

*** =instructions
ให้คุณคำนวณค่าสัมประสิทธิ์ส่วนเบี่ยงเบนมาตรฐานของ `n_checkins`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# `mean()` and `sd()` of `n_checkins` are here
mean_checkins <- mean(chain_checkin_summary$n_checkins)
sd_checkins <- sd(chain_checkin_summary$n_checkins)

# calculate `CV` here

```

*** =solution
```{r}
# `mean()` and `sd()` of `n_checkins` are here
mean_checkins <- mean(chain_checkin_summary$n_checkins)
sd_checkins <- sd(chain_checkin_summary$n_checkins)

# calculate `CV` here
sd_checkins / mean_checkins
```

*** =sct
```{r}
success_msg("Cool!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:9d04a53efc
## Interquantile Range

`Interquantile Range` เป็นอีกวิธีหนึ่งในการวัดการกระจายตัวของข้อมูล แทนที่จะใช้ส่วนเบี่ยงเบนมาตรฐานที่พิจารณาความผันผวนจากทุกจุดข้อมูล `Interquantile Range` จะทำการพิจารณาแค่ระยะห่างจากข้อมูล ณ percentile ที่ 25 ไปยังข้อมูล ณ percentile ที่ 75 แทน ดังนั้นจึงมีอีกชื่อเรียกหนึ่งว่า `midspread` หรือ `middle 50%`

*** =instructions
ให้คุณใช้ function `IQR()` กับคอลัมน์ `n_checkins` ของตัวแปร `chain_checkin_summary` เพื่อหา `Interquantile Range` ของจำนวนเช็คอินของร้านอาหารแต่ละแบรนด์

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# use `IQR()` with `chain_checkin_summary$n_checkins` here!

```

*** =solution
```{r}
# use `IQR()` with `chain_checkin_summary$n_checkins` here!
IQR(chain_checkin_summary$n_checkins)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:e3b4411559
## Histogram (1)

ในแบบฝึกหัดนี้เราก็ยังคงอยู่กับ `chain_checkin_summary` แต่เราจะกลับมาโฟกัสที่การทำ data visualization กันอีกครั้ง!

คราวนี้เราจะสร้างสิ่งที่เรียกว่า `Histogram` ซึ่งเอาไว้ใช้ดูการกระจายตัวของข้อมูลเชิงปริมาณได้ แกน `x` บน `Histogram` จะทำหน้าที่แทนช่วงของค่าต่างๆที่เป็นไปได้จากข้อมูลของเรา ส่วนแกน `y` จะทำหน้าที่แทนความถี่ (หรือจำนวนครั้ง) ที่ข้อมูลในแต่ละช่วงแกน `x` นั้นปรากฎขึ้น เมื่อนำมาประกอบกัน เราก็จะได้กราฟที่แสดงการแจกแจงความถี่ของข้อมูลแต่ละช่วง

*** =instructions

- ให้คุณสร้าง `layer` แรกด้วย function `ggplot()` และใช้ `x = n_checkins` เป็น argument ของ function `aes()` เพื่อบอก R ว่าเราจะใช้คอลัมน์ `n_checkins` เป็นข้อมูลในการสร้างกราฟต่อไป
- จากนั้นสร้าง `layer` ต่อมาด้วย function `geom_histogram()` เพื่อระบุว่ากราฟที่เราต้องการคือ `Histogram`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# create your first `Histogram` using `ggplot()` and `geom_histogram()` here
chain_checkin_summary %>%
  ggplot(aes(...)) + ...
```

*** =solution
```{r}
# create your first `Histogram` using `ggplot()` and `geom_histogram()` here
chain_checkin_summary %>%
  ggplot(aes(x = n_checkins)) + geom_histogram()
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:b6941c3fee
## Histogram (2)

กราฟ `Histogram` อาจมีหน้าตาแตกต่างกันได้ขึ้นอยู่กับความกว้างของช่วงต่อแท่งข้อมูลที่เรากำหนด ซึ่งคุณสามารถกำหนดความกว้างของการแบ่งช่วงได้โดยการกำหนดค่า argument `binwidth` ลงไปใน function `geom_histogram()`

นอกจากนี้ เรายังสามารถเปลี่ยนสีของกราฟได้เหมือนกับกราฟรูปแบบอื่นๆ argument `color` และ `fill` จะเป็นตัวกำหนดสีของขอบแท่งข้อมูลและตัวแท่งข้อมูลตามลำดับ

*** =instructions

- เพิ่ม argument `binwidth = 2` ลงไปเพื่อให้ข้อมูลแต่ละช่วงมีความกว้างเท่ากับ `2`
- เพิ่ม argument `color = "white"` เพื่อให้ขอบแท่งข้อมูลเปลี่ยนเป็นสีขาว
- เพิ่ม argument `fill = "#48b6a3"` เพื่อให้ตัวแท่งข้อมูลเปลี่ยนเป็นสีฟ้า

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# let's tidy up things by decrease the histogram binwidth and change some colours!
chain_checkin_summary %>%
  ggplot(aes(x = n_checkins)) + geom_histogram(..., ..., ...)
```

*** =solution
```{r}
# let's tidy up things by decrease the histogram binwidth and change some colours!
chain_checkin_summary %>%
  ggplot(aes(x = n_checkins)) + geom_histogram(binwidth = 2, color = "white", fill = "#48b6a3")
```

*** =sct
```{r}
success_msg("Bravo!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c31a78926d
## Histogram (3)

เราสามารถเพิ่มเติม `layer` เข้าไปอีกเพื่อแสดงรายละเอียดอื่นๆได้ด้วยเช่นกัน ตัวอย่างเช่น

- function `geom_vline()` จะทำการเพิ่มเส้นแนวตั้งลงไปในกราฟ ซึ่งอาจเป็นประโยชน์ในกรณีที่คุณต้องการแสดงเส้นข้อมูลต่างๆเพิ่มเติม เช่น เส้นค่าเฉลี่ยหรือเส้นข้อมูล ณ percentile ต่างๆ
- function `geom_text()` ช่วยในการเพิ่มตัวอักษรลงไปบนกราฟ ทำให้คุณสามารถเขียนอธิบายข้อมูลเพิ่มเติมได้ถ้าต้องการ

*** =instructions
ให้คุณเพิ่ม `layer` ใหม่อีก 2 `layer` โดยใช้คำสั่งที่เตรียมไว้ให้ดังนี้

- `geom_vline(aes(xintercept = mean(n_checkins)), color = "#1E4865", linetype = "longdash")` เพื่อเพิ่มเส้นประสีน้ำเงินในแนวตั้ง แสดงจุดค่าเฉลี่ยของ `n_checkins`
- `geom_text(data = data.frame(), aes(x = mean_checkin + 2, y = 150, label = paste("Mean =", round(mean_checkin, digits = 2))), hjust = 0, size = 5)` เพื่อเพิ่มข้อความสำหรับอธิบายเส้นแนวตั้งที่เราเพิ่มเข้าไปอีกที โดย argument `x` และ `y` ใน function `aes()` จะทำหน้าที่แทนคู่อันดับ บอกตำแหน่งที่ข้อความจะปรากฎขึ้น

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# let's add some more information to the graph using `geom_vline()` and `geom_text()` functions
chain_checkin_summary %>%
  ggplot(aes(x = n_checkins)) + 
    geom_histogram(binwidth = 2, color = "white", fill = "#48b6a3") +
    ... +
    ...
```

*** =solution
```{r}
# let's add some more information to the graph using `geom_vline()` and `geom_text()` functions
chain_checkin_summary %>%
  ggplot(aes(x = n_checkins)) + 
    geom_histogram(binwidth = 2, color = "white", fill = "#48b6a3") +
    geom_vline(aes(xintercept = mean(n_checkins)), color = "#1E4865", linetype = "longdash") +
    geom_text(data = data.frame(), 
      aes(x = mean_checkin + 2, y = 150, label = paste("Mean =", round(mean_checkin, digits = 2))), 
      hjust = 0, 
      size = 5
    )
```

*** =sct
```{r}
success_msg("Good Job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:6e2835a1db
## Histogram (4)

หากคุณต้องการปรับขนาดของแกน `x` ให้แสดงค่าเฉพาะในขอบเขตที่คุณต้องการ รวมถึงการแสดงค่าแกน `x` ก็สามารถทำได้โดยการซ้อน `layer` `scale_x_continuous()`

คำสั่ง `scale_x_continuous(breaks = seq(0, 100, 10))` จะทำการเปลี่ยนให้กราฟแสดงค่าแกน `x` ตั้งแต่ `0` ถึง `100` โดยแสดงค่า `x` ทีละ `10`

เพื่อให้เห็นภาพมากขึ้น เรามาลองเพิ่ม `layer` `scale_x_continuous()` ให้ `Histogram` ในแบบฝึกหัดที่แล้วกัน

*** =instructions

- ตั้งชื่อให้กราฟ, แกน `x` และแกน `y` ว่า "Checkin Distribution (by chain)", "Number of Checkins" และ "Count" ตามลำดับ
- เพิ่ม `layer` `scale_x_continuous()` เพื่อกำหนดให้ R แสดงค่าแกน `x` ตั้งแต่ `0` ถึง `160` โดยแบ่งแสดงค่า `x` ทีละ `20`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# add `labs()` and `scale_x_continuous()` layers to the graph
chain_checkin_summary %>%
  ggplot(aes(x = n_checkins)) + 
    geom_histogram(binwidth = 2, color = "white", fill = "#48b6a3") +
    geom_vline(aes(xintercept = mean(n_checkins)), color = "#1E4865", linetype = "longdash") +
    geom_text(data = data.frame(), 
      aes(x = mean_checkin + 2, y = 150, label = paste("Mean =", round(mean_checkin, digits = 2))), 
      hjust = 0, 
      size = 5
    ) +
    labs(title = ..., x = ..., y = ...) +
    ...
```

*** =solution
```{r}
# add `labs()` and `scale_x_continuous()` layers to the graph
chain_checkin_summary %>%
  ggplot(aes(x = n_checkins)) + 
    geom_histogram(binwidth = 2, color = "white", fill = "#48b6a3") +
    geom_vline(aes(xintercept = mean(n_checkins)), color = "#1E4865", linetype = "longdash") +
    geom_text(data = data.frame(), 
      aes(x = mean_checkin + 2, y = 150, label = paste("Mean =", round(mean_checkin, digits = 2))), 
      hjust = 0, 
      size = 5
    ) +
    labs(title = "Checkin Distribution (by chain)", x = "Number of Checkins", y = "Count") +
    scale_x_continuous(breaks = seq(0, 160, 20))
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:eb39da766e
## Box Plot (1)

data visualization อีกรูปแบบหนึ่งที่ช่วยให้เราสามารถดูการกระจายตัวของข้อมูลได้ก็คือ `Box Plot`

`Box Plot` จะแสดงค่า `5-number summary` เอาไว้ในกราฟ รวมถึงจะแสดง `outlier` ออกมาอย่างชัดเจนด้วย

ในการสร้าง `Box Plot` คุณจะต้องใช้ function `geom_boxplot()` เพื่อสร้าง `layer` `Box Plot` ต่อจาก `layer` `ggplot()`

*** =instructions

- สร้าง `layer` `ggplot()` โดยใช้ `aes(x = "Check-ins", y = n_checkins)` เป็น argument เพื่อให้ R ใช้คอลัมน์ `n_checkins` เป็นข้อมูลในการสร้าง `Box Plot`
- สร้าง `Box Plot` โดยซ้อน `geom_boxplot()` เข้าไปอีกหนึ่ง `layer`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# create `Box Plot` using `ggplot()` and `geom_boxplot()` functions
chain_checkin_summary %>%
  ... +
  ...
```

*** =solution
```{r}
# create `Box Plot` using `ggplot()` and `geom_boxplot()` functions
chain_checkin_summary %>%
  ggplot(aes(x = "Check-ins", y = n_checkins)) +
  geom_boxplot()
```

*** =sct
```{r}
success_msg("Good Job!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:5e3e09920e
## Box Plot (2)

คล้ายกับตอนคุณสร้างแผนภูมิแท่ง คุณสามารถทำการปรับแต่งความกว้างของ `Box Plot` และสลับแกน `x` และ `y` ได้เช่นกัน

*** =instructions

- เพิ่ม argument `width = 0.1` ลงไปใน function `geom_boxplot()`
- เพิ่ม `layer` `coord_flip()` ต่อท้าย function `geom_boxplot()` เพื่อสลับแกน `Box Plot`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
chain <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/chain.tsv", encoding = "UTF-8")
restaurant_checkin_user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant_checkin_user.tsv")

chain_checkin_summary <- restaurant %>%
  filter(
    chain_id != 0, 
    domain_id == 1
  ) %>%
  select(id, chain_id) %>%
  left_join(restaurant_checkin_user, by = c("id" = "restaurant_id")) %>%
  group_by(chain_id) %>% 
  summarise(
    n_restaurants = n_distinct(id),
    n_checkins = sum(checkins)
  ) %>%
  inner_join(chain, by = c("chain_id" = "id")) %>%
  select(chain_id, n_restaurants, n_checkins, name) %>%
  mutate(n_checkins = ifelse(is.na(n_checkins), 0, n_checkins)) %>%
  arrange(-n_checkins)
```

*** =sample_code
```{r}
# add `width = 0.1` into function `geom_boxplot()` and add a new layer `coord_flip()`
chain_checkin_summary %>%
  ggplot(aes(x = "Check-ins", y = n_checkins)) +
  geom_boxplot(...) +
  ...
```

*** =solution
```{r}
# add `width = 0.1` into function `geom_boxplot()` and add a new layer `coord_flip()`
chain_checkin_summary %>%
  ggplot(aes(x = "Check-ins", y = n_checkins)) +
  geom_boxplot(width = 0.1) +
  coord_flip()
```

*** =sct
```{r}
success_msg("Good Job!")
```
