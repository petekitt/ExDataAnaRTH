---
title meta  : บทที่ 2
title       : Exploratory Data Analysis on One Variable - Part1
description : Insert the chapter description here
--- type:NormalExercise lang:r xp:100 skills:1 key:f055d83ad0
## เรามาลองสำรวจข้อมูลผู้ใช้งานกันอีกหน่อย (1)

ทวนความจำเรื่องการใช้ factor เพื่อเปลี่ยนแปลงรูปแบบข้อมูลให้เหมาะสมกับการวิเคราะห์ข้อมูลประเภท categorical มากขึ้น

*** =instructions
ในตอนนี้เรามี data frame `user` ซึ่งเก็บข้อมูลของผู้ใช้งานเว็บไซต์อยู่ใน workspace ให้คุณกด Submit เพื่อลองดูผลของการใช้ function `factor` กับคอลัมน์ `gender` จาก data frame `user`

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

นอกจาก package `dplyr` ที่ช่วยในการจัดการข้อมูลแล้ว เราก็ยังมี package `ggplot2` ซึ่งช่วยในการทำ data visualization หลากหลายรูปแบบ ไม่ว่าจะเป็น `Bar Chart`, `Histogram`, `Boxplot`, `Scatter Plot` และอื่นๆอีกมากมาย

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

ปกติแล้วถ้า

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
# create the first layer with `ggplot()` function here
ggplot(data = user, mapping = aes(x = gender))
```

*** =solution
```{r}
# create the first layer with `ggplot()` function here
ggplot(data = user, mapping = aes(x = gender)) + geom_bar()
```

*** =sct
```{r}
success_msg("Yureka! You can see that the visualization was built using a simple `geom_bar` layer but this is not all we can do.")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:4b82ef22be
## Bar Chart (3)

อย่างที่คุณเห็นในแบบฝึกหัดที่แล้ว ถ้าข้อมูลที่คุณใส่ลงไปใน function `ggplot()` ไม่ได้มีการจัดรูปแบบให้เหมาะสม R จะทำการสร้างแผนภูมิโดยให้แกน `y` แทนจำนวนนับของข้อมูลใน dataset ที่มีค่าตามแกน `x`

คุณสามารถเปลี่ยนแปลงค่าเหล่านั้นได้โดยการระบุแกน `y` ให้ชัดเจน คุณอาจระบุให้แกน `y` มีค่าเป็น `count` เพื่อให้แสดงค่า `y` ตามการนับจำนวนเหมือนเดิม หรืออาจระบุให้แกน `y` แทนคอลัมน์ต่างๆใน dataset เพื่อให้ R แสดงค่าออกมาตามนั้นก็ได้ เพียงแต่ข้อมูลที่คุณใส่ลงไปควรจะถูกจัดให้อยู่ในรูปแบบที่เหมาะสม

ดังนั้น คราวนี้เราจะเปลี่ยนข้อมูลเป็น `gender_summary` ซึ่งถูกเตรียมไว้จากแบบฝึกหัดก่อนๆหน้านี้แทน เพื่อให้การสร้างแผนภูมิแท่งสามารถสื่อความหมายได้ดีขึ้น

*** =instructions
- เปลี่ยน argument `data` ให้มีค่าเป็นตัวแปร `gender_summary` แทนที่จะเป็น `user`
- นอกจากการกำหนดแกน `x = gender` แล้ว ให้คุณเพิ่ม `y = count` ลงไปด้วยเพื่อระบุให้ชัดเจนว่าคุณต้องการให้ R นับข้อมูลตามแกน `x`
- ให้คุณระบุ argument `stat = "identity"` ลงไปภายใน function `geom_bar()` มิเช่นนั้น R จะไม่สามารถแสดงภาพแผนภูมิออกมาได้

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
# create the first layer with `ggplot()` function here
ggplot(data = user, mapping = aes(x = gender, ...)) + geom_bar(...)
```

*** =solution
```{r}
# create the first layer with `ggplot()` function here
ggplot(data = gender_summary, mapping = aes(x = gender, y = count)) + geom_bar(stat = "identity")
```

*** =sct
```{r}
success_msg("Good! The graph is now a bit more informative and you can modify the function even more!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:846659ff03
## Bar Chart (4)


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

--- type:NormalExercise lang:r xp:100 skills:1 key:ad4e0721cc
## Bar Chart (5)


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

--- type:NormalExercise lang:r xp:100 skills:1 key:fe1746b0ca
## Bar Chart (6)


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

--- type:NormalExercise lang:r xp:100 skills:1 key:7362d84d34
## Bar Chart (7)


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

--- type:NormalExercise lang:r xp:100 skills:1 key:1e67f78696
## Bar Chart (8)


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
