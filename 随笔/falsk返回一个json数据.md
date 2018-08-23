falsk返回一个json数据

    @blue.route('/getjson/')
    def getjson():
        data = {
            'main': 'hello flask',
            'status': 200
        }
        return jsonify(data)