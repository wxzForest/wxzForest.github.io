# AirTest图像识别算法


#### Airtest图像识别算法

> 1、模板匹配（templateMatching）
> 2、基于特侦点的图像识别算法（SURF Matching、BRISK Matching）
> > 模板匹配
> > 1、模板匹配无法跨分辨率识别。
> > 2、一定有相对最佳的匹配结果（一定会给一个匹配结果出来，即使相差很大）。
> > 3、方法名：tpl。

>> 特侦点识别
>> 1、跨分辨率识别。
>> 2、不一定有匹配结果。
>> 3、方法名：[kaze，brisk，akaze，orb，sift，surf，brief]

airtest使用中实际会采用轮询的方式去匹配，具体为先采用模板匹配查询，再采用特侦点匹配查询，直到查询出一个最优解。

```python
# 使用BRISK算法匹配
try match with BRISKMatching
find_best_result() run time is 0.21 s.
match result: None
resize: (160, 208)->(160, 208), resolution: (1080, 2400)=>(1080, 1920)
try match with SURFMatching
find_best_result() run time is 0.23 s.
try match with TemplateMatching

# confidence置信度只有0.399所以结果被舍弃了
[Template] threshold=0.7, result={'result': (123, 1285), 'rectangle': ((43, 1181), (43, 1389), (203, 1389), (203, 1181)), 'confidence': 0.3993876576423645}
find_best_result() run time is 0.08 s.
try match with BRISKMatching
find_best_result() run time is 0.22 s.
match result: None
resize: (160, 208)->(160, 208), resolution: (1080, 2400)=>(1080, 1920)
try match with SURFMatching
'Target area is 5 times bigger or 0.2 times smaller than sch_img.'
try match with TemplateMatching
[Template] threshold=0.7, result={'result': (123, 1285), 'rectangle': ((43, 1181), (43, 1389), (203, 1389), (203, 1181)), 'confidence': 0.3993876576423645}
find_best_result() run time is 0.08 s.
try match with BRISKMatching
find_best_result() run time is 0.21 s.
match result: None
resize: (160, 208)->(160, 208), resolution: (1080, 2400)=>(1080, 1920)
try match with SURFMatching
```
