---
title: 第一次用 Flex 切版就上手
date: 2018-01-01 17:52:46
tags: [css, flex]
---

# 前言

Flex 是在 CSS3 發佈的屬性，目的是為了讓大家使用 flex 後能更有效率的達成想要的切版效果，像是大家在排版的時候，是不是常常會遇到垂直置中的問題呢？因此有很多人會特別整理出 「垂直置中的 X 個方法」這類型的文章，而今天要介紹的 Flex 排版法，則是其中一種可以方便大家在切版時隨時設定垂直對齊的排版方法。那麼，我們就直接來介紹 Flex 的使用方式吧！

# Flex 的屬性使用教學

## 設定父層的 display 為 flex 或 inline-flex

`display: flex | inline-flex;`

要使用 Flex 排版的物件，首先父層的 display 一定要設定為 flex 或 inline-flex，這樣父層裡的子元件才能吃到 flex 的屬性設定，而屬性值 inline-flex 則能讓父層保有 inline-block 的特性。


## 利用 flex 設定父層內的子元件大小（flex-grow、flex-shrink 和 flex-basis）

### flex-grow
flex-grow 是指當父層的總空間分配給所有子元件後，如果空間還有剩下來的話，設定了 flex-grow 的子元件可以從剩下的空間裡額外得到多少比例的空間，在沒有設定之前 flex-grow 的預設值為 0，表示在沒有設定的狀態下子元件都只會照自己原本的元件空間顯示，不會去佔多餘的父層空間，但是當有子元件被設定了 flex-grow 的時候，父層剩下的空間則會依照子元件們 flex-grow 的總和去分配每個元件額外可以分到多少空間，例如父層的寬度是 300px，而父層的下面有三個子元素，分別是 .a .b 和 .c，這三個子元素的寬度各是 50px，當 flex-grow 沒有設定屬性值或屬性值為 0 的時候，那 .a .b 和 .c 佔據的寬度就會是他們原始的寬，也就是 50px，但當 .a 設定 `flex-grow: 1;` 時，父層剩下的空間將會分成一等分分給 .a，所以實際呈現的樣子就會是：

- 父層剩下的空間 = 300px - 50px(.a 的寬) - 50px(.b 的寬) - 50px(.c 的寬) = 150px
- .a 的寬 = 50px(.a 原始寬) + 150px(父層剩下的空間) = 200px
- .b 和 .c 則維持原本的 50px 不變

而當我們設定了 .a 的 flex-grow 為 2，.b 的 flex-grow 為 1 時，則會像下面這樣：

- 父層剩下的空間 = 300px - 50px(.a 的寬) - 50px(.b 的寬) - 50px(.c 的寬) = 150px
- 每一份 flex-grow 所佔據的寬 = 150 / 3(.a 的 `flex-grow: 2;` + .b 的 `flex-grow: 1;`) = 50px
- .a 的寬 = 50px(.a 原始寬) + 50px * 2(有兩份 flex-grow) = 150px
- .b 的寬 = 50px(.b 原始寬) + 50px * 1(有一份 flex-grow) = 100px
- .c 的寬維持原本的 50px 不變

### flex-shrink
若說 flex-grow 代表的是子元件的伸展性，那 flex-shrink 則是指子元件的收縮性，當父元素的空間不夠放入所有子元件時，flex-shrink 的數值就控制了子元件是否能夠被收縮（預設值為 0，也就是不能收縮），以及整體收縮的比例是多少（屬性值越大則收縮的幅度越大）。

### flex-basis
flex-basis 指的則是子物件的最小寬度或高度（這裡的指的是寬或高是由 flex-direction 的設定值來決定，flex-direction 將在本文後面詳述），依照上述的例子 .a .b 和 .c 的原始寬度 50px 就是利用此屬性進行設定。

而上述的三種屬性可以簡寫在一個 flex 屬性內，寫法是： `flex: flex-grow的值 flex-shrink的值 flex-basis的值 ;`。


## 在父層設定 flex-flow 決定子元素的排序方向還有是否換行（flex-direction 和 flex-wrap）

### flex-direction 可以決定子元件的排序方向

`flex-direction: row | row-reverse | column | column-reverse;`

我們可以從父層設定子層物件的排列方式，在沒有設定的狀態下預設的屬性值是 row （使物件由左至右排序），而 row-reverse 則能使物件以相反方向排序（由右至左），如果希望物件是以垂直的方式編排，我們則會使用屬性值 column（使物件由上至下排序）或是 column-reverse（使物件由下至上排序）進行排序。

### flex-wrap 則決定了子物件是否可以換行

`flex-wrap: nowrap | wrap | wrap-reverse;`

flex-wrap 預設的屬性為 nowrap 不換行，wrap 為換行，wrap-reverse 為換行且排序方式與 wrap 相反。

flex-direction 和 flex-wrap 可以簡寫為 flex-flow，寫法為：`flex-flow: flex-direction的值 || flex-wrap的值`。


## 子物件的對齊設定

### justify-content

`justify-content: flex-start | flex-end | center | space-between | space-around;`

justify-content 會依照我們設定的 flex-direction 方向，移動子物件的對齊位置，以 `flex-direction: row;`為例，當我們設定 `justify-content: flex-start;` 時子物件會向左靠齊、flex-end 會向右靠齊、center 會使子物件置中對齊、space-between 會讓第一個子物件靠左，第二個子物件靠右，並使剩下的物件等距對齊，而 space-around 則會在左右都不靠齊父層的狀況下等距對齊所有子元素。

### align-items

`align-items: flex-start | flex-end | center | baseline | stretch;`

align-items 對齊的方向與 justify-content 相反，像當 `flex-direction: row;` 的時候 justify-content 控制的是子元素的左右對齊，那 align-items 就是控制子元素跟子元素之間的上下對齊，反之，當 `flex-direction: column;` 的時候 justify-content 控制的是子元素的上下對齊，那 align-items 就是控制子元素跟子元素之間的左右對齊，我們這裡一樣以 `flex-direction: row;` 為例，當我們設定 `align-items: flex-star;` 時子元素就會向上對齊、flex-end 時會向下對齊、center 時會置中對齊、baseline 時會以物件的基準線為主進行對齊，而 stretch 則可以自動將子元素的高度延展成一樣的高度。

### align-content

`align-content: flex-start | flex-end | center | space-between | space-around | stretch;`

align-content 是多行的屬性，當子元素的數量多到換行時 align-content 會對整個子元素區塊設定對齊方式，而屬性值代表的意思與上述的屬性值大多相同，故此不再復述。


# 結語

透過 flex 的對齊特性，我們可以用更方便的方式佈局我們要切的畫面，希望這篇文章能幫助大家在未來實作時能有更多切版屬性的選擇，如果對文章有任何問題也歡迎提出來一起討論。
