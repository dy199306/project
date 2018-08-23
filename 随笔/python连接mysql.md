### 1、python连接mysql

    import pymysql
    db =pymysql.Connect(host='localhost',port=3306,user='root',password='dy123321',database='day01')
    cursor = db.cursor()
    cursor.execute('SELECT VERSION()')
    data = cursor.fetchone()
    print('Database Version: %s' % data)
    #输出结果 Database Version: 5.7.23-0ubuntu0.16.04.1
    db.close()
### 添加数据

    db = pymysql.Connect(host='localhost', port=3306, user='root', password='dy123321', database='day01')
    cursor = db.cursor()
    urls = main()
    for u in urls:
    	#数据格式化的两种方式
        # cursor.execute('insert into movies(url) values(%s)',[u])
        cursor.execute('insert into movies(url) values(%s)',(u,))
    # data = cursor.fetchone()
        db.commit()
    db.close()
### 查询数据

    db = pymysql.Connect(host='localhost', port=3306, user='root', password='dy123321', database='day01')
    cursor = db.cursor()
    cursor.execute('select * from movies')
    data = cursor.fetchall()
    print(data)
    # db.commit()
    db.close()
### 删除数据

    db = pymysql.Connect(host='localhost', port=3306, user='root', password='dy123321', database='day01')
    cursor = db.cursor()
    try:
        cursor.execute('delete from movies where id=1')
        print('删除成功')
        db.commit()
    except:
        print('删除失败')    
    db.close()
### 修改数据

    db = pymysql.Connect(host='localhost', port=3306, user='root', password='dy123321', database='day01')
    cursor = db.cursor()
    try:
        cursor.execute('update movies set url="你好啊" where id=2')
        print('修改成功')
        db.commit()
    except:
        print('修改失败')
    db.close()