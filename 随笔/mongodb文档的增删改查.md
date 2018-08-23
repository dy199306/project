### mongodb文档的增删改查



##### 1、增加文档

insert插入

    1.插入一条数据
        格式: db.集合名.insert(数据)  数据要写成bson(类似json格式)形式
    例:db.students.insert({age:69,name:'安倍晋三',gender:0,address:"东京热"})
    2.插入多条数据
        格式: db.集合名.insert([{},{},{}]) 插入多条数据用[] 括起来
        db.students.insert([{age:66,name:'三胖胖',gender:1,address:"平壤"},{age:55,name:'普京',gender:1,address:"莫斯科"},{age:33,name:'马蓉',gender:0,address:"东京热"}])
    3.save 插入/保存 数据
       格式:与insert类似  
         db.students.save({age:66,name:'蔡引文',gender:0,address:"台湾省"})
         指定一个id,
     用save时,如果该_id存在,则表示修改
         db.students.save({"_id" : ObjectId("5b45745ae597f42ec9504c5f"),age:66,name:'小菜',gender:1,address:"台湾省xx"})
         用insert时,如果该_id存在,则会报错,不能修改
         db.students.insert({"_id" : ObjectId("5b45745ae597f42ec9504c5f"),age:66,name:'小小小小菜',gender:1,address:"台湾省xx"})














































修改文档

       例: db.students.update({name:'小菜'},{age:88})   注意: 这是将整条数据改成 age:88

       修改: $set  表示在原有的数据上进行修改  
       	     db.students.update({name:'安倍狗'},{$set:{age:88}})
       增加: $inc 表示在原有的值上进行 增加字段 操作
    //当使用$inc修改器时，当字段不存在时，会自动创建该字段，如果存在，则在原有值的基础上进行增加或者减少
    //$inc主要是用于专门进行数字的增加或减少，因此$inc只能用于整型，长整形，或者双精度浮点型的值
    //$inc不支持字符串，数组以及其他非数字的值
    //注，对于$inc的操作，$set也可以完成。$inc存在的理由是$inc更高效
            db.students.update({name:'安倍狗'},{$inc:{age:11}})
    
      upsert:true, 表示如果没有找到对应的数据,是否将该修改的数据当作新的数据插入   true表示是 插入  ,  false(默认值) 否, 不插入
      
      multi:true    表示是否对多有的匹配项都进行修改, false 默认的, 默认只会修改第一个匹配项,   true表示将所有的匹配项都修改


      例:  db.students.update({name:'小yy'},{age:88,name:'小yy'},{upsert:false}) 
      例:  db.students.update({address:'东京热'},{$set:{gender:0}},{multi:true})
    $set 和 upsert一起使用
      db.students.update({age:70},{$set:{address:"中国"}},{upsert:true})
      如果找不到匹配的{age:70} 则插入{age:70}和{address:"中国"}两个文档
    单独upsert
      db.students.update({age:75},{address:"莫斯科"},{upsert:true})
      如果找不到匹配的{age:70} 则插入{address:"中国"}一个文档
    