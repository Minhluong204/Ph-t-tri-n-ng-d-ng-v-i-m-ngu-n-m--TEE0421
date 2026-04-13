# D.Bonus
## 1. Tạo thư mục `./myapi`
```
cd ~/myapp
mkdir -p ./myapi
```
## 2. Tạo file `./myapi/app.py`
Tạo file :
```
nano ./myapi/app.py
```

<img width="500" height="64" alt="image" src="https://github.com/user-attachments/assets/99c41ab2-5d9b-4f2c-8bb7-b9da5def5437" />

 Dán nội dung :
 ```
from flask import Flask, jsonify, request
import random
import datetime

app = Flask(__name__)

jokes = [
    "Tại sao lập trình viên thích mùa đông? Vì có nhiều 'bug' cần fix 😄",
    "Code không chạy? Đừng lo, đó là feature chưa được khám phá!",
    "Tôi không lười, tôi chỉ đang chạy ở chế độ tiết kiệm năng lượng.",
    "Debugging: Là nghệ thuật xóa bug mà mình vừa tạo ra.",
]

@app.route('/')
def home():
    return jsonify({
        "message": "Welcome to Funny API 🤡",
        "time": str(datetime.datetime.now())
    })

@app.route('/joke')
def get_joke():
    return jsonify({
        "joke": random.choice(jokes)
    })

@app.route('/roast', methods=['GET'])
def roast():
    name = request.args.get('name', 'bạn')
    roasts = [
        f"{name} code như viết văn nghị luận 😏",
        f"{name} chạy code mà bug chạy nhanh hơn 🏃",
        f"{name} debug 2 tiếng, fix bằng restart 😆",
    ]
    return jsonify({
        "roast": random.choice(roasts)
    })

@app.route('/luck')
def luck():
    percent = random.randint(0, 100)
    return jsonify({
        "luck": f"Hôm nay bạn may mắn {percent}% 🍀"
    })

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000, debug=True)

```

