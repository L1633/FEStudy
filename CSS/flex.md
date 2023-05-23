## flex
flex: 1
等价于 
flex: 1 1 auto

flex: flex-grow flex-shrink flex-basis|auto|initial|inherit;
flex-grow	一个数字，规定项目将相对于其他灵活的项目进行扩展的量。
flex-grow属性定义项目的放大比例，默认为0，即如果存在剩余空间，也不放大。
如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。

flex-shrink	一个数字，规定项目将相对于其他灵活的项目进行收缩的量。
flex-shrink属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。
如果所有项目的flex-shrink属性都为1，当空间不足时，都将等比例缩小。如果一个项目的flex-shrink属性为0，其他项目都为1，则空间不足时，前者不缩小。
负值对该属性无效。

flex-basis	项目的长度。合法值："auto"、"inherit" 或一个后跟 "%"、"px"、"em" 或任何其他长度单位的数字。


flex-wrap 属性规定flex容器是单行或者多行，同时横轴的方向决定了新行堆叠的方向。
nowrap	默认值。规定灵活的项目不拆行或不拆列。
wrap	规定灵活的项目在必要的时候拆行或拆列。