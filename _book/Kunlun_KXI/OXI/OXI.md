# OXI

# AllotmentHelper

## 1.KXI上行allotment消息具体过程:

#### 一.首先通过 HotelValidate 校验

需要酒店code,消息类型,接口类型

#### 二.判断是否为多房价: 

如果为多房价单独存储 parRateCodeList 集合

AllotmentRateCodes 节点下包含 AllotmentRateCodeCollection 节点各个房价

#### 三.通过 owner 节点和 hotelcode 节点查询销售员: 

code如果不存在取默认值 ,如果没有owner节点仍取默认值.

#### 四.处理档案: 

AssociatedProfiles 节点下包含 ProfileCollection 节点各个档案信息

通过 ProfileType 节点筛选档案类型

通过 mfResortProfileID 节点前台编号 ,查询并判断档案是否存在 , 不存在就insert

*insert档案时需要获取 ProfileCollection 节点所有数据,酒店code,上行所有节点数据

#### 五.档案中包含关联关系:

如果 Relationships 节点中有数据,则获取 Relationship 节点中 Relations节点下 Relation 节点各个关联关系

在dic_relationships表中写入关联关系

#### 六.检查锁房数据:

检查 InventoryBlocks 节点下 InventoryBlockCollection 节点各个锁房数据是否开启 Elastic 弹性锁房 

Elastic默认为 0 

#### 七.获取团队预定id:

通过前台编号,酒店code,mfAllotmentId(此为团队预定id)  查询block_id.

#### 八.检查宴会状态:

检查 Catering 节点中的 StatusCode 节点状态代码是否存在, 如果不存在判断是否配置了转换,如果没配置转换就取默认值.

#### 九.检查渠道:

检查 mfBlockOriginatorCode 节点渠道代码是否存在,如果不存在判断是否配置了转换,如果没配置转换就取默认值.

#### 十.检查市场:

检查 mfMarketCode 节点市场代码是否存在,如果不存在判断是否配置了转换,如果没配置转换就取默认值.

#### 十一.检查多房价:

检查 RatePlanCodes 节点单房价或多房价 parRateCodeList 集合中代码是否存在,如果不存在判断是否配置了转换,如果没配置转换就取默认值.

#### 十二.检查付款方式: 

PaymentMethod 节点 , 其他同上

#### 十三.检查来源: 

mfSourceCode 节点 , 其他同上

#### 十四.检查预定状态: 

mfBookingStatus 节点 , 其他同上

#### 十五.检查取消原因: 

CancelCode 节点 , 其他同上 

#### 十六.检查预定类型: 

mfReservationType 节点 , 其他同上

#### 十七.判断主档案:

通过市场代码,来源代码,销售员代码,房价代码,酒店code查询所有档案id,
通过档案id获取主档案的档案类型

#### 十八.insert或update团队预定并添加修改日志:

判断block_id是否存在,insert或update团队预定 , update团队预定会添加修改日志.

#### 十九.返回result消息:

判断block_id是否存在,生成消息体包含,团队预定id,团队预定pmsid,档案id,档案pmsid(程序会+1).

#### 二十.检查IMP_BLOCK_CONTACT系统参数判断是否处理联系人:

如果 Relationships 节点中有数据,则获取 Relationship 节点中 Relations节点下 Relation 节点各个关联关系
如果存在联系人关联关系,先删除block_cust_contact_relaction_info表中数据再新增联系人关联关系,否则直接增加

#### 二十一.处理锁房数据:

获取 InventoryBlocks 节点下 InventoryBlockCollection 节点各个锁房数据
先delete后insert

#### 二十二.处理备注信息:

获取 AllotmentNotes 节点下 AllotmentNoteCollection 节点各个备注信息
检查 AllotmentNoteCollection 节点中的 NoteType是否存在,如果不存在判断是否配置了转换,如果没配置转换就取默认值.

#### 二十三.最后处理完毕