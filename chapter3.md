---
title meta  : บทที่ 3
title       : Exploratory Data Analysis on Two Variable
description : ในบทนี้เราจะพาคุณไปเรียนรู้เพิ่มเติมเกี่ยวกับการสำรวจข้อมูล 2 ตัวแปรพร้อมๆกัน ซึ่งจะช่วยให้เราวิเคราะห์ข้อมูลได้ฝนมิติที่กว้างขึ้น ลึกขึ้น และเป็นประโยชน์มากขึ้น!
--- type:NormalExercise lang:r xp:100 skills:1 key:807fcae9f0
## ร้านอาหารกับบัตรเครดิต (1)

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
## ร้านอาหารกับบัตรเครดิต (2)

จากแบบฝึกหัดที่แล้ว จะเห็นได้ว่า function `table()` สามารถนับจำนวนตัวแปรแบบ `categorical` และจัดกลุ่มตามค่าของข้อมูลได้ด้วย แต่รูปแบบการสรุกข้อมูลจากการใช้ function `table()` อาจจะยังไม่เหมาะสมต่อการนำไปทำ data visualization

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
## ร้านอาหารกับบัตรเครดิต (3)

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

ลองสร้างแผนภูมิแท่งอีกรอบจากข้อมูลอัตราส่วน!

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

นับตั้งแต่แบบฝึกหัดนี้ เราจะเริ่มเรียนรู้เกี่ยวกับการวิเคราะห์ข้อมูล Categorical ร่วมกับข้อมูลเชิงปริมาณหรือ Continuous กันบ้าง โดยที่เราจะกลับมาดูที่ข้อมูลคะแนนร้านอาหารกันบ้าง

*** =instructions

- คำนวณคะแนนเฉลี่ยของร้านอาหารแต่ละร้านโดยใช้คอลัมน์ `rating` จาก data frame `rating` แบ่งกลุ่มตามคอลัมน์ `reviewed_item_id`
- ใช้ function `inner_join()` เพื่อเชื่อมผลลัพธ์จากด้านบนเข้ากับ data frame `restaurant` โดยให้คอลัมน์ `reviewed_item_id` เท่ากับ `id` และอย่าลืมว่าคุณต้องใช้ double quotes (`"`) ในการระบุชื่อคอลัมน์ในการเชื่อม data frame
- ใช้ function `filter()` เพื่อคัดกรองเฉพาะข้อมูลร้านอาหารที่มี `chain_id` เท่ากับ `1` หรือ `454` หรือ `457` เท่านั้น (นั่นคือร้าน `Starbucks`, `After You`, และ `Bonchon`)

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
```

*** =sample_code
```{r}
# start rearrange data from `rating` and `restaurant` here!
rating %>%
  group_by(...) %>%
  summarise(avg_rating = ...) %>%
  inner_join(restaurant, by = c(... = ...)) %>%
  filter(chain_id == ... | chain_id == ... | chain_id == ...)
```

*** =solution
```{r}
# start rearrange data from `rating` and `restaurant` here!
rating %>%
  group_by(reviewed_item_id) %>%
  summarise(avg_rating = mean(rating)) %>%
  inner_join(restaurant, by = c("reviewed_item_id" = "id")) %>%
  filter(chain_id == 1 | chain_id == 454 | chain_id == 457)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:8df04b2892
## การวิเคราะห์ตัวแปรแบบ Categorical ร่วมกับตัวแปรแบบ Continuous (2)


*** =instructions

- ใช้ function `select()` เพื่อเลือกเฉพาะคอลัมน์ `chain_id` และ `avg_rating` จากผลลัพธ์ในแบบฝึกหัดที่แล้ว
- ใช้ function `mutate()` เพื่อสร้างคอลัมน์ใหม่ชื่อ `chain_name` โดยกำหนดให้มีค่าเท่ากับ `factor(chain_id, levels = c(1, 454, 457), labels = c("Starbucks", "After You", "Bonchon"))`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
```

*** =sample_code
```{r}
# use `select()` and `mutate()` to finalize the data before plotting the graph
rating %>%
  group_by(reviewed_item_id) %>%
  summarise(avg_rating = mean(rating)) %>%
  inner_join(restaurant, by = c("reviewed_item_id" = "id")) %>%
  filter(chain_id == 1 | chain_id == 454 | chain_id == 457) %>%
  select(..., ...) %>%
  mutate(chain_name = ...)
```

*** =solution
```{r}
# use `select()` and `mutate()` to finalize the data before plotting the graph
rating %>%
  group_by(reviewed_item_id) %>%
  summarise(avg_rating = mean(rating)) %>%
  inner_join(restaurant, by = c("reviewed_item_id" = "id")) %>%
  filter(chain_id == 1 | chain_id == 454 | chain_id == 457) %>%
  select(chain_id, avg_rating) %>%
  mutate(chain_name = factor(chain_id, levels = c(1, 454, 457), labels = c("Starbucks", "After You", "Bonchon")))
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:acf4a87be0
## การวิเคราะห์ตัวแปรแบบ Categorical ร่วมกับตัวแปรแบบ Continuous (3)

เราสามารถสร้าง `Box Plot` หลายๆอันพร้อมกันได้ โดยแบ่งแท่ง `Box Plot` ตามกลุ่มข้อมูลซึ่งเราได้แบ่งไว้แล้วในแบบฝึกหัดก่อนๆ

*** =instructions

- สร้าง `layer` `ggplot()` โดยให้แกน `x` และ `y` มีค่าเป็นคอลัมน์ `chain_name` และ `avg_rating` ตามลำดับ
- สร้าง `layer` `geom_boxplot()` โดยใส่ argument `width = 0.2` เพื่อกำหนดขนาดแท่งข้อมูล `Box Plot`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
restaurant <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/restaurant.tsv", encoding = "UTF-8")
rating <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/rating.tsv")
```

*** =sample_code
```{r}
# create `ggplot()` and `geom_boxplot()` layers to create boxplots!
rating %>%
  group_by(reviewed_item_id) %>%
  summarise(avg_rating = mean(rating)) %>%
  inner_join(restaurant, by = c("reviewed_item_id" = "id")) %>%
  filter(chain_id == 1 | chain_id == 454 | chain_id == 457) %>%
  select(chain_id, avg_rating) %>%
  mutate(chain_name = factor(chain_id, levels = c(1, 454, 457), labels = c("Starbucks", "After You", "Bonchon"))) %>%
  ggplot(aes(x = ..., y = ...)) + ...
```

*** =solution
```{r}
# create `ggplot()` and `geom_boxplot()` layers to create boxplots!
rating %>%
  group_by(reviewed_item_id) %>%
  summarise(avg_rating = mean(rating)) %>%
  inner_join(restaurant, by = c("reviewed_item_id" = "id")) %>%
  filter(chain_id == 1 | chain_id == 454 | chain_id == 457) %>%
  select(chain_id, avg_rating) %>%
  mutate(chain_name = factor(chain_id, levels = c(1, 454, 457), labels = c("Starbucks", "After You", "Bonchon"))) %>%
  ggplot(aes(x = chain_name, y = avg_rating)) + geom_boxplot(width = 0.2)
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

- คำนวณจำนวน `rating` ทั้งหมดของ `user` แต่ละคนโดยการรวมคอลัมน์ `n_1_ratings` ถึง `n_5_ratings` แล้วเก็บค่าไว้ในคอลัมน์ `n_ratings_correct`
- ใช้ function `cor()` กับคอลัมน์ `n_likes` และ `n_ratings_correct` เพื่อหาค่าความสัมพันธ์เชิงเส้น (`Correlation`) ระหว่างการกดไลค์และการให้คะแนนร้านอาหาร

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
user <- read.delim("http://s3.amazonaws.com/assets.datacamp.com/production/course_3635/datasets/user.tsv")
```

*** =sample_code
```{r}
# use `cor()` with `user$n_likes` and `user$n_ratings_correct` here
user$n_ratings_correct <- user$n_1_ratings + ... + ... + ... + ...
cor(..., ...)
```

*** =solution
```{r}
# use `cor()` with `user$n_likes` and `user$n_ratings_correct` here
user$n_ratings_correct <- user$n_1_ratings + user$n_2_ratings + user$n_3_ratings + user$n_4_ratings + user$n_5_ratings
cor(user$n_likes, user$n_ratings_correct)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:750b0d0198
## Correlation (2)

คราวนี้เราจะมาลองดู dataset ที่ถูกเตรียมไว้ให้โดยโปรแกรม R กันบ้าง (built-in dataset) dataset ดังกล่าวก็คือ `anescombe`

`anscombe` เป็นชื่อย่อของ `Anscombe's Quartet` ซึ่งเป็นชื่อเรียก dataset 4 กลุ่ม ที่มีตัวเลขทั่วไปทางสถิติ (descriptive statistics) ใกล้เคียงกันมากแต่จะมีความแตกต่างที่เห็นได้ชัดเจนเมื่อคุณนำ dataset เหล่านี้มาสร้างกราฟ

*** =instructions

- ให้คุณพิมพ์ `anscombe` ลงไปเพื่อตรวจสอบดูว่า dataset มีหน้าตาเป็นอย่างไร
- ใช้ function `cor()` กับคอลัมน์ `x1` และ `y1` ไปจนถึง `x4` และ `y4` ของ data set `anscombe` เพื่อตรวจสอบค่า `correlation` ของคู่อันดับ `x` และ `y` ต่างๆ

*** =hint

*** =pre_exercise_code
```{r}

```

*** =sample_code
```{r}
# print out the data set here to see how it looks like
...

# display correlation between x's and y's
cor(anscombe$x1, anscombe$y1)
cor(..., ...)
cor(..., ...)
cor(anscombe$x4, anscombe$y4)
```

*** =solution
```{r}
# print out the data set here to see how it looks like
anscombe

# display correlation between x's and y's
cor(anscombe$x1, anscombe$y1)
cor(anscombe$x2, anscombe$y2)
cor(anscombe$x3, anscombe$y3)
cor(anscombe$x4, anscombe$y4)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:cd200ef225
## Scatter Plot (1)

จากแบบฝึกหีดที่แล้ว จะเห็นได้ว่า dataset ทั้ง 4 นั้นมีค่า `correlation` ที่ใกล้เคียงกันมาก ซึ่งเราอาจถูกหลอกว่าข้อมูลมีลักษณะเหมือนกันได้จากการพิจารณาแค่ค่าทางสถิติบางตัว นี่จึงเป็นการแสดงให้เห็นถึงความสำคัญของการทำ data visualiztion เพื่อให้คุณเข้าใจลักษณะของข้อมูลมากขึ้น

ในแบบฝึกหัดนี้ เราจะมาสร้างสิ่งที่เรียกว่า `Scatter Plot` กัน `Scatter Plot` คือการสร้างกราฟคู่อันดับระหว่างแกน `x` และ `y` เพื่อมองหาความสัมพันธ์ของทั้งสองแกน โดยคู่อันดับแต่ละคู่จะถูกแสดงผลออกมาในรูปจุด ซึ่งเราสามารถใช้ function `ggplot()` ร่วมกับ `geom_point()` ได้ในการสร้าง `Scatter Plot` ใน R

*** =instructions

- สร้าง `layer` `gpplot()` จากตัวแปร `anscombe` โดยให้คอลัมน์ `x1` และ `y1` เป็นแกน `x` และแกน `y` ตามลำดับ
- สร้าง `layer` `geom_point()` โดยใส่ argument `colour = "#48b6a3"` และ `size = 3` เพื่อกำหนดสีและขนาดให้กับจุดข้อมูล

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
```

*** =sample_code
```{r}
# create your first `Scatter Plot` using `ggplot()` and `geom_point()` here!
anscombe %>%
  ggplot(aes(x = ..., y = ...)) + ...
```

*** =solution
```{r}
# create your first `Scatter Plot` using `ggplot()` and `geom_point()` here!
anscombe %>%
  ggplot(aes(x = x1, y = y1)) + geom_point(colour = "#48b6a3", size = 3)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:fefd09ebe3
## Scatter Plot (2)

เพื่อแสดงให้เห็นว่า `x` และ `y` แต่ละคู่ตั้งแต่ 1 ถึง 4 มีความสัมพันธ์แตกต่างกันจริงๆ เราจะทำการสร้าง `Scatter Plot` พร้อมๆกัน 4 กราฟ

แต่ก่อนที่จะทำการสร้างกราฟได้ เราจะต้องจัดข้อมูลให้อยู่ในรูปแบบที่เหมาะสมเสียก่อน โดยการนำข้อมูลมาต่อกันให้เหลือสองคอลัมน์ ได้แก่ `x` และ `y` จากนั้นเพิ่มคอลัมน์ที่ 3 เพื่อระบุว่าคู่อันดับ `x` และ `y` แต่ละคู่อยู่ในกลุ่ม dataset ที่เท่าไร

*** =instructions

- ใช้ function `select()` เพื่อเลือกคอลัมน์ `x1` และ `y1` จาก `anscombe` ตั้งชื่อคอลัมน์ว่า `x` และ `y` ตามลำดับ
- ใช้ function `mutate()` เพื่อเพิ่มคอลัมน์ใหม่ชื่อ `group` และกำหนดให้คอลัมน์นี้มีค่าเท่ากับ `1` ทั้งคอลัมน์

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
```

*** =sample_code
```{r}
# use `select()` and `mutate()` to start arranging an appropriate format for the data 
anscombe %>%
  select(x = ..., y = ...) %>%
  mutate(group = ...)
```

*** =solution
```{r}
# use `select()` and `mutate()` to start arranging an appropriate format for the data 
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:1bed58614d
## Scatter Plot (3)

คุณสามารถนำ data frame 2 อันหรือมากกว่ามาต่อกันได้โดยการใช้ function `bind_rows()` โดยที่มีเงื่อนไขว่า data frame ที่นำมาต่อกันจะต้องมีชื่อคอลัมน์ตรงกันและข้อมูลในคอลัมน์นั้นๆต้องเป็นประเภทเดียวกัน

*** =instructions
ใช้ function `bind_rows()` ร่วมกับ `data.frame()` เพื่อจัดรูปแบบข้อมูลของคู่อันดับ `x1` และ `y1` ไปจนถึง `x4` และ `y4` ให้เหมาะสม

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
```

*** =sample_code
```{r}
# connect the data from all the remaining group row-wised 
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = ..., y = ..., group = ...),
    data.frame(x = ..., y = ..., group = ...)
  )
```

*** =solution
```{r}
# connect the data from all the remaining group row-wised 
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = anscombe$x3, y = anscombe$y3, group = 3),
    data.frame(x = anscombe$x4, y = anscombe$y4, group = 4)
  )
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:74057da207
## Scatter Plot (4)

ในตอนนี้เราได้ข้อมูลในรูปแบบที่เราต้องการแล้ว ดังนั้นเราจะมาเริ่มสร้าง `Scatter Plot` กัน!

*** =instructions

- ให้คุณสร้าง `layer` `ggplot()` โดยให้แกน `x` และแกน `y` มีค่าเท่ากับคอลัมน์ `x` และคอลัมน์ `y`
- สร้าง `layer` `geom_point()` โดยกำหนด argument `colour = "#48b6a3"` และ `size = 3`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
```

*** =sample_code
```{r}
# create `Scatter Plot` on your rearranged data 
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = anscombe$x3, y = anscombe$y3, group = 3),
    data.frame(x = anscombe$x4, y = anscombe$y4, group = 4)
  ) %>%
  ggplot(aes(x = ..., y = ...)) +
    geom_point(..., ...)
```

*** =solution
```{r}
# create `Scatter Plot` on your rearranged data 
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = anscombe$x3, y = anscombe$y3, group = 3),
    data.frame(x = anscombe$x4, y = anscombe$y4, group = 4)
  ) %>%
  ggplot(aes(x = x, y = y)) +
    geom_point(colour = "#48b6a3", size = 3)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:c0233375c2
## Scatter Plot (5)

อย่างไรก็ตาม คุณจะเห็นได้ว่า `Scatter Plot` ในแบบฝึกหัดที่แล้วยังไม่ได้มีการแบ่งกลุ่มตามคอลัมน์ `group` ซึ่งนั่นไม่ใช่สิ่งที่เราต้องการ

ถ้าคุณยังจำได้ ในการแยกกราฟออกจากกัน เราต้องใช้ function `facet_wrap()`

*** =instructions
เพิ่ม `layer` `facet_wrap()` และใส่ `~ group` เป็น argument เพื่อบอกให้ R แบ่งกราฟออกตามคอลัมน์ `group`

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
```

*** =sample_code
```{r}
# add `facet_wrap()` `layer` to separate, by group, the current `Scatter Plot` into multiple graphs
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = anscombe$x3, y = anscombe$y3, group = 3),
    data.frame(x = anscombe$x4, y = anscombe$y4, group = 4)
  ) %>%
  ggplot(aes(x = x, y = y)) +
    geom_point(colour = "#48b6a3", size = 3) +
    ...
```

*** =solution
```{r}
# add `facet_wrap()` `layer` to separate, by group, the current `Scatter Plot` into multiple graphs
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = anscombe$x3, y = anscombe$y3, group = 3),
    data.frame(x = anscombe$x4, y = anscombe$y4, group = 4)
  ) %>%
  ggplot(aes(x = x, y = y)) +
    geom_point(colour = "#48b6a3", size = 3) +
    facet_wrap(~ group)
```

*** =sct
```{r}
success_msg("Great!")
```

--- type:NormalExercise lang:r xp:100 skills:1 key:ec4b4efd91
## Scatter Plot (6)

เห็นไหมว่า data set แต่ละกลุ่มมีรูปร่างหน้าตาแตกต่างกันโดยสิ้นเชิง แม้จะมีค่าทางสถิติเบื้องต้นที่ใกล้เคียงกันมากก็ตาม

นอกจากกราฟสร้างกราฟธรรมดา เรายังสามารถสร้าง `layer` ที่ทำหน้าที่แสดงเส้นแนวโน้มเพื่อให้เราตีความข้อมูลได้ง่ายขึ้นได้อีกด้วย function ที่เราต้องใช้เพื่อสร้างเส้นแนวโน้มก็คือ `geom_smooth()` โดยเราสามารถเลือกวิธีการสร้างเส้นแนวโน้มได้ด้วยการระบุค่า argument `method`

นอกจากนี้เราก็สามารถระบุขนาดของเส้นและรูปแบบของเส้นได้ด้วย argument `size` และ `linetype` เช่นกัน

*** =instructions
เพิ่ม `layer` `geom_smooth()` พร้อม argument `method = "lm"`, `se = FALSE`, `size = 0.5`, `linetype = "dashed"` เพื่อเพิ่มเส้นแนวโน้มแบบเส้นประลงบนกราฟ `Scatter Plot` ทุกกราฟ

*** =hint

*** =pre_exercise_code
```{r}
library("dplyr")
library("ggplot2")
```

*** =sample_code
```{r}
# add `geom_smooth()` to add trendlines to the `Scatter Plots`
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = anscombe$x3, y = anscombe$y3, group = 3),
    data.frame(x = anscombe$x4, y = anscombe$y4, group = 4)
  ) %>%
  ggplot(aes(x = x, y = y)) +
    geom_point(colour = "#48b6a3", size = 3) +
    facet_wrap(~ group) +
    ...(method = ..., se = ..., size = ..., linetype = ...)
```

*** =solution
```{r}
# add `geom_smooth()` to add trendlines to the `Scatter Plots`
anscombe %>%
  select(x = x1, y = y1) %>%
  mutate(group = 1) %>%
  bind_rows(data.frame(x = anscombe$x2, y = anscombe$y2, group = 2),
    data.frame(x = anscombe$x3, y = anscombe$y3, group = 3),
    data.frame(x = anscombe$x4, y = anscombe$y4, group = 4)
  ) %>%
  ggplot(aes(x = x, y = y)) +
    geom_point(colour = "#48b6a3", size = 3) +
    facet_wrap(~ group) +
    geom_smooth(method = "lm", se = FALSE, size = 0.5, linetype = "dashed")
```

*** =sct
```{r}
success_msg("Great!")
```
