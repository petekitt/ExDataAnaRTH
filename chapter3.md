---
title meta  : บทที่ 3
title       : Exploratory Data Analysis on Two Variable
description : ในบทนี้เราจะพาคุณไปเรียนรู้เพิ่มเติมเกี่ยวกับการสำรวจข้อมูล 2 ตัวแปรพร้อมๆกัน ซึ่งจะช่วยให้เราวิเคราะห์ข้อมูลได้ฝนมิติที่กว้างขึ้น ลึกขึ้น และเป็นประโยชน์มากขึ้น!
--- type:NormalExercise lang:r xp:100 skills:1 key:807fcae9f0
## ร้านอาหารและบัตรเครดิต (1)

ในยุคปัจจุบัน เราปฏิเสธไม่ได้ว่าบัตรเครดิตก็เข้ามามีส่วนสำคัญในการใช้ชีวิตของเราด้วยเช่นกัน ซึ่งในการไปรับประทานอาหารหรูราคาแพงบางมื้อ เราก็อาจจะยินดีจ่ายค่าอาหารด้วยบัตรเครดิตมากกว่าเงินสด

ตัวแปร `restaurant` นอกจากจะมีการเก็บรายละเอียดทั่วไปอย่างเช่น ชื่อและที่อยู่ร้านอาหารแล้ว ก็ยังมีการเก็บข้อมูลว่าร้านแต่ละร้านรับชำระค่าอาหารด้วยบัตรเครดิตหรือไม่อีกด้วย ดังนั้นในช่วงต้นของบทนี้ เราจะมาทำการวิเคราะห์ข้อมูลเกี่ยวกับการชำระเงินด้วยบัตรเครดิตของร้านอาหารในแต่ละช่วงราคากัน

*** =instructions
ให้คุณใช้ function `table()` กับคอลัมน์ `price_range` และ `credit_card_accepted` จาก data frame `restaurant` เพื่อดูว่ามีจำนวนร้านอาหารกี่ร้านที่รับชำระเงินด้วยบัตรเครดิต แบ่งตามช่วงราคาอาหาร

*** =hint

*** =pre_exercise_code
```{r}
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# use `table()` function here!
table(restaurant$..., restaurant$...)
```

*** =solution
```{r}
# use `table()` function here!
table(restaurant$price_range, restaurant$credit_card_accepted)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:56e10e4591
## ร้านอาหารและบัตรเครดิต (2)

จากแบบฝึกหัดที่แล้ว จะเห็นได้ว่า function `table()` สามารถนับจำนวนตัวแปรแบบ categorical และจัดกลุ่มตามค่าของข้อมูลได้ด้วย แต่รูปแบบการสรุกข้อมูลจากการใช้ function `table()` อาจจะยังไม่เหมาะสมต่อการนำไปทำ data visualization

ดังนั้น เราจะมาทำการจัดรูปแบบข้อมูลให้เหมาะสมมากขึ้นด้วยการใช้ package `dplyr`

function `count()` สามารถช่วยในการนับจำนวนแถวข้อมูลโดยแบ่งกลุ่มตามคอลัมน์ที่ใส่ลงไปเป็น argument (เหมือนกับการ `group_by()` แล้วตามด้วย `summarise()` โดยการใช้ `n()`)

*** =instructions

- `filter()` เฉพาะร้านอาหารที่ `price_range != 0` เพื่อกรองร้านอาหารที่ไม่ได้มีการระบุช่วงราคาอาหารที่ชัดเจนออกไป
- ใช้ function `count()` กับ `restaurant` โดยใส่ argument เป็นคอลัมน์ `price_range` และ  `credit_card_accepted` ตามลำดับ

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# `count()` numbers of observations in `restaurant` separately by `price_range` and `credit_card_accepted`
restaurant %>%
  ... %>%
  ...
```

*** =solution
```{r}
# `count()` numbers of observations in `restaurant` separately by `price_range` and `credit_card_accepted`
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:f6bf4c2f30
## ร้านอาหารและบัตรเครดิต (3)

จากที่เห็นในผลลัพธ์ คอลัมน์ `price_range` จะมีค่าตั้งแต่ 1 ถึง 5 โดยที่ 1 หมายถึงร้านอาหารที่มีราคาถูกที่สุด ในขณะที่ 5 หมายถึงร้านอาหารที่มีราคาแพงที่สุด

และคอลัมน์ `credit_card_accepted` จะมีค่าเป็น 0 หรือ 1 เท่านั้น ซึ่ง 0 หมายถึงไม่รับบัตรเครดิต และ 1 หมายถึงรับบัตรเครดิต

เนื่องจากค่า 0 และ 1 จากคอลัมน์ `credit_card_accepted` อาจจะดูไม่ค่อยสื่อความหมายเท่าไร ดังนั้นเราจะมาทำการจัดรูปแบบข้อมูลให้สื่อความหมายมากขึ้นโดยการใช้ function `factor()`

*** =instructions

- ใช้ function `ungroup()` กับผลลัพธ์ในแบบฝึกหัดที่แล้วเพื่อยกเลิกการจัดกลุ่มของคอลัมน์ `price_range` และ `credit_card_accepted` (เพราะเราไม่สามารถใช้ function `mutate()` กับคอลัมน์ที่มีการจัดกลุ่มจะได้)
- เปลี่ยนคอลัมน์ `price_range` และ `credit_card_accepted` ให้กลายเป็นข้อมูลประเภท `factor` โดยการใส่ `price_range = factor(price_range)` และ `credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept")` เป็น argument ของ function `mutate()`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# use `ungroup()` and `mutate()` function
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ... %>%
  ...
```

*** =solution
```{r}
# use `ungroup()` and `mutate()` function
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  )
```

*** =sct
```{r}
success_msg("Great! Now that you get a proper form of data, we will move to the data visualization step!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:9a972ead51
## Bar Chart อีกครั้ง (1)

เมื่อเราได้ข้อมูลในรูปแบบที่เราต้องการแล้ว ยังจำสิ่งที่ได้เรียนไปในบทที่แล้วได้ไหม เราจะมาสร้างแผนภูมิแท่งกัน!

*** =instructions

- นำผลลัพธ์จากแบบฝึกหัดที่แล้วมาสร้าง `layer` `ggplot()` โดยใช้คอลัมน์ `price_range` เป็นแกน `x` ใช้คอลัมน์ `n` (ซึ่งแทนจำนวนนับของร้านอาหารในแต่ละกลุ่ม) เป็นแกน `y` และแบ่งสีตามคอลัมน์ `credit_card_accepted` ด้วย argument `fill`
- สร้าง `layer` `geom_bar()` โดยใส่ argument `stat = "identity"` และ `width = 0.4`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# create `ggplot()` and `geom_bar()` layers in accordance of the instructions
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = ..., y = ..., fill = ...)) +
  ...
```

*** =solution
```{r}
# create `ggplot()` and `geom_bar()` layers in accordance of the instructions
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = price_range, y = n, fill = credit_card_accepted)) +
  geom_bar(stat = "identity", width = 0.4)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:422b3852ec
## Bar Chart อีกครั้ง (2)

จากแบบฝึกหีดที่แล้ว คุณจะเห็นว่าแท่งข้อมูลนั้นซ้อนกันอยู่ เราเรียกการแสดงผลแบบนี้ว่า stacked ความสูงของแท่งข้อมูลที่ซ้อนกันสุดท้ายแล้วจะมีค่าเท่ากับจำนวนร้านอาหารทั้งหมดใน `price_range` นั้นๆ

เราสามารถทำให้แผนภูมิแท่งดูได้ง่ายขึ้นโดยการแยกแท่งข้อมูลที่ซ้อนกันอยู่ออกจากกัน ซึ่งในการที่จะทำแบบนั้นได้ เราต้องเปลี่ยนค่า argument `position` ใน function `geom_bar()`

*** =instructions
เพิ่ม argument `position = "dodge"` ลงไปใน function `geom_bar()` เพื่อให้แท่งข้อมูลไม่ทับกัน

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# add argument `position = "dodge"` to the `geom_bar()` function
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = price_range, y = n, fill = credit_card_accepted)) +
  geom_bar(stat = "identity", width = 0.4)
```

*** =solution
```{r}
# add argument `position = "dodge"` to the `geom_bar()` function
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = price_range, y = n, fill = credit_card_accepted)) +
    geom_bar(stat = "identity", width = 0.4, position = "dodge")
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:64426203f6
## Bar Chart อีกครั้ง (3)

ในกรณีที่คุณต้องการกำหนดสีให้กับแท่งข้อมูลด้วยตัวเอง ก็สามารถทำได้โดยการใช้ function `scale_fill_manual()` เพื่อเลือกค่าสีของแท่งข้อมูลต่างๆโดยการกำหนดค่าให้ argument `values`

*** =instructions

- เพิ่ม `layer` `scale_fill_manual(values = c("#db9b39", "#48b6a3"))` เพื่อกำหนดสีให้กับแท่งข้อมูลแต่ละแท่งด้วยตัวคุณเอง
- เพิ่ม `layer` `coord_flip()` เพื่อสลับแกน `x` และแกน `y`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# overlay `scale_fill_manual()` and `coord_flip()` functions
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = price_range, y = n, fill = credit_card_accepted)) +
    geom_bar(stat = "identity", width = 0.4, position = "dodge") +
    ... +
    ...
```

*** =solution
```{r}
# overlay `scale_fill_manual()` and `coord_flip()` functions
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = price_range, y = n, fill = credit_card_accepted)) +
    geom_bar(stat = "identity", width = 0.4, position = "dodge") +
    scale_fill_manual(values = c("#db9b39", "#48b6a3")) +
    coord_flip()
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:7518a31e14
## Bar Chart อีกครั้ง (4)

การสร้างแผนภูมิแท่งในแบบฝึกหัดที่ผ่านๆมา อาจสามารถใช้บอกได้แค่จำนวนของร้านอาหารในแต่ละกลุ่ม แต่หากเราต้องการทำความเข้าใจข้อมูลมากขึ้นเพื่อหาความสัมพันธ์ระหว่างช่วงราคาอาหารและการรับชำระบัตรเครดิต เราอาจจะพิจารณาข้อมูลโดยใช้อัตราส่วนแทน

ข้อสันนิษฐานเบื้องต้นเกี่ยวกับการรับชำระด้วยบัตรเครดิตคือ ร้านอาหารที่มีช่วงราคาอาหารสูงกว่า น่าจะมีอัตราส่วนการรับชำระด้วยบัตรเครดิตที่มากกว่า เราจะไปวิเคราะห์ข้อมูลที่เรามีว่าเป็นเช่นนั้นจริงหรือไม่

*** =instructions

- จัดกลุ่มข้อมูลตาม `price_range` ด้วย function `group_by()`
- สร้างคอลัมน์ `pct` ด้วย function `mutate()` โดยให้ `pct = n * 100 / sum(n)` เพื่อคำนวณอัตราส่วนร้านอาหารที่รับและไม่รับชำระด้วยบัตรเครดิตแบ่งตาม `price_range`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# calculate percentage of credit-card-accepted restaurant for each `price_range` using `group_by()` and `mutate()`
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ... %>%
  ...
```

*** =solution
```{r}
# calculate percentage of credit-card-accepted restaurant for each `price_range` using `group_by()` and `mutate()`
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  group_by(price_range) %>%
  mutate(pct = n * 100 / sum(n))
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:9379c919a5
## Bar Chart อีกครั้ง (5)

ลองสร้างแผนภูมิแท่งอีกรอบจากข้อมูบอัตราส่วน!

*** =instructions

- เปลี่ยนค่าที่นำมาสร้างแกน `y` ใน `layer` `ggplot()` จากคอลัมน์ `n` เป็น `pct`
- ลบ `position = "dodge"` ใน `layer` `geom_bar()` ออกเพื่อให้แท่งข้อมูลกลับมาซ้อนกันและแสดงผลของข้อมูลแบบอัตราส่วนได้ดีขึ้น

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# calculate percentage of credit-card-accepted restaurant for each `price_range` using `group_by()` and `mutate()`
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  group_by(price_range) %>%
  mutate(pct = n * 100 / sum(n)) %>%
  ggplot(aes(x = price_range, y = n, fill = credit_card_accepted)) +
    geom_bar(stat = "identity", width = 0.4, position = "dodge") +
    scale_fill_manual(values = c("#db9b39", "#48b6a3")) +
    coord_flip()
```

*** =solution
```{r}
# calculate percentage of credit-card-accepted restaurant for each `price_range` using `group_by()` and `mutate()`
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  group_by(price_range) %>%
  mutate(pct = n * 100 / sum(n)) %>%
  ggplot(aes(x = price_range, y = pct, fill = credit_card_accepted)) +
    geom_bar(stat = "identity", width = 0.4) +
    scale_fill_manual(values = c("#db9b39", "#48b6a3")) +
    coord_flip()
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:aa45e44276
## Facetting

หากคุณต้องการสร้างชุดแผนภูมิแท่งหลายๆชุดที่แยกออกจากกัน R ก็สามารถทำได้เช่นกัน ในกรณีนี้คุณจะต้องใช้ `layer` `facet_wrap()`

function `facet_wrap()` จะทำการสร้างกราฟทีละหลายๆกราฟโดยแบ่งกลุ่มตาม argument ที่คุณใส่ลงไป เพื่อให้เห็นภาพมากขึ้น ลองไปดูแบบฝึกหัดกันเลย

*** =instructions

- สร้าง `layer` `ggplot()` โดยให้แกน `x` มีค่าเป็นคอลัมน์ `credit_card_accepted` และแกน `y` มีค่าเป็นคอลัมน์ `n`
- สร้าง `layer` `geom_bar()` โดยให้ `stat = "identity"`, `fill = "#48b6a3"` และ `width = 0.5`
- สร้าง `layer` `facet_wrap()` โดยใช้คอลัมน์ `price_range` เป็นตัวแบ่ง facet และให้ `scales = "free"`
- สร้าง `layer` `labs()` โดยกำหนดชื่อ `x = ""`, `y = "Count"` และ `title = "Accepting Credit Cards vs. Price Range"`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
```

*** =sample_code
```{r}
# create all the layers as stated in the instructions
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = ..., y = ...)) + 
    geom_bar(stat = ..., fill = ..., width = ...) +
    facet_wrap(~ ..., scales = ...) +
    labs(x = ..., y = ..., title = ...)
```

*** =solution
```{r}
# create all the layers as stated in the instructions
restaurant %>%
  filter(price_range != 0) %>%
  count(price_range, credit_card_accepted) %>%
  ungroup() %>%
  mutate(
    price_range = factor(price_range), 
    credit_card_accepted = factor(credit_card_accepted, levels = c(0, 1), labels = c("Not accept", "Accept"))
  ) %>%
  ggplot(aes(x = credit_card_accepted, y = n)) + 
    geom_bar(stat = "identity", fill = "#48b6a3", width = 0.5) +
    facet_wrap(~ price_range, scales = "free") +
    labs(x = "", y = "Count", title = "Accepting Credit Cards vs. Price Range")
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:5368a20948
## การวิเคราะห์ตัวแปรแบบ Categorical ร่วมกับตัวแปรแบบ Continuous (1)

หากคุณลองสังเกต จะเห็นได้ว่าข้อมูลช่วงราคาอาหารและการรับชำระด้วยบัตรเครดิตที่เราได้วิเคราะห์ไปในแบบฝึกหัดก่อนๆนั้น เป็นข้อมูลประเภทแบ่งกลุ่มหรือ Categorical ทั้งคู่

นับตั้งแต่แบบฝึกหัดนี้ เราจะเริ่มเรียนรู้เกี่ยวกับการวิเคราะห์ข้อมูล Categorical ร่วมกับข้อมูลเชิงปริมาณหรือ Continuous กันบ้าง

*** =instructions
ใน Workspace มีข้อมูลผู้ใช้งานเก็บไว้ในตัวแปร `user` ซึ่งเราจะนำมาใช้ในการวิเคราะห์ข้อมูลในแบบฝึกหัดต่อไป ถ้าพร้อมแล้วก็กดปุ่ม `submit` เพื่อดูโครงสร้างคร่าวๆของตัวแปร `user` ได้เลย

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# click `submit` when you are ready
str(user)
```

*** =solution
```{r}
# click `submit` when you are ready
str(user)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:8df04b2892
## การวิเคราะห์ตัวแปรแบบ Categorical ร่วมกับตัวแปรแบบ Continuous (2)


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
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:d0a3fcbddf
## การวิเคราะห์ตัวแปรแบบ Categorical ร่วมกับตัวแปรแบบ Continuous (3)


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
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:acf4a87be0
## การวิเคราะห์ตัวแปรแบบ Categorical ร่วมกับตัวแปรแบบ Continuous (4)


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
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:e05e7640f1
## Correlation (1)

เราสามารถดูความสัมพันธ์เชิงเส้นระหว่างตัวแปรสองตัวได้โดยใช้ค่าที่เรียกว่า `Correlation`

ค่า `Correlation` จะมีค่าอยู่ระหว่าง -1 ถึง 1 หากมีค่าเป็นบวกแสดงว่าตัวแปรสองตัวมีความสัมพันธ์ไปในทางเดียวกัน แต่หากเป็นลบก็แสดงว่าตัวแปรสองตัวมีความสัมพันธ์ไปในทิศทางตรงกันข้าม

และค่า `Correlation` ยังสามารถใช้บอกความแข็งแรงของความสัมพันธ์เชิงเส้นได้ด้วย หากยิ่งมีค่าใกล้ -1 หรือ 1 แสดงว่าความสัมพันธ์ของตัวแปรคู่นี้มีความแข็งแรงมาก แต่หากใกล้ 0 ก็แสดงว่ามีความสัมพันธ์น้อยมาก หรือไม่มีความสัมพันธ์เชิงเส้นต่อกัน

*** =instructions
ลองใช้ function `cor()` กับคอลัมน์ `n_likes` และ `n_ratings_correct` เพื่อหาค่าความสัมพันธ์เชิงเส้น (`Correlation`) ระหว่างการกดไลค์และการให้คะแนนร้านอาหาร

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# use `cor()` with `user$n_likes` and `user$n_ratings_correct` here

```

*** =solution
```{r}
# use `cor()` with `user$n_likes` and `user$n_ratings_correct` here
cor(user$n_likes, user$n_ratings_correct)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:750b0d0198
## Correlation (2)


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
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:cd200ef225
## Scatter Plot (1)


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
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:fefd09ebe3
## Scatter Plot (2)


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
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:1bed58614d
## Scatter Plot (3)


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
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:74057da207
## Scatter Plot (4)


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
success_msg("Great!")
```
