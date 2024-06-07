<!DOCTYPE html>>
<html>
<head>
    <style>
        #char{
                width: 40px;
                height: 40px;
                background-color: blue;
                position: relative;
            }
        .gun{
                width: 30px;
                height: 30px;
                border-radius: 30%;
                background-color: black;
                position: absolute;
            }
        #coordinates {
            position: fixed;
            bottom: 10px;
            left: 10px;
            background-color: white;
            padding: 5px;
            border: 1px solid #ccc;
           }
        #ground {
            width: 1000px;
            height: 1000px;
            
            border: 10px solid black;
            background-color: rgba(95, 158, 160, 0.1);
        }
    </style>
</head>
<body>
    <div id="ground">
        <div id="char"></div>
        <!-- 실시간 좌표를 표시할 요소 -->
        <div id="coordinates">
            X: <span id="x-coordinate"></span>,
            Y: <span id="y-coordinate"></span>,
            V: <span id="Velocity-coordinate"></span>
        </div>
        <!-- JavaScript 불러오기-->
        <script src="script.js"></script>        
    </div>
</body>
</html>
<script>
    // script.js
    // 실시간 좌표 업데이트
    function locationChar(){
        document.getElementById('x-coordinate').innerText = x;
        document.getElementById('y-coordinate').innerText = y;
        document.getElementById('Velocity-coordinate').innerText = Math.sqrt(dx * dx + dy * dy);
    }
</script>
<!-- Char -->
<script>
    //char
    const char = document.getElementById('char');
    let x = 0;
    let y = 0;
    let dx = 0;
    let dy = 0;
    // char의 속도
    let speed = 5;
    // char의 위치
    function moveChar() {    
        x += dx;
        y += dy;
        char.style.left = x + 'px';
        char.style.top = y + 'px';
        requestAnimationFrame(moveChar);
        // 위치가 변경될 때마다 좌표 업데이트 호출
        locationChar();
    }
    //char 기능
    window.addEventListener('keydown',function(event) {
        switch(event.key) {
            case 'ArrowUp' :
                dy = -speed;
                dx = 0;
                lastDirection = 'up';
            break;
            case 'ArrowDown' :
                dy = +speed;
                dx = 0;
                lastDirection = 'down';
            break;
            case 'ArrowLeft' :
                dx = -speed;
                dy = 0;
                lastDirection = 'left';
            break;
            case 'ArrowRight' :
                dx = +speed;
                dy = 0;
                lastDirection = 'right';gg
            break;
            case 'S':
            case 's':
            stop(); // stop 함수 호출
            break;
            
            // F키를 눌러 100px 이동
            case 'f':
            case 'F':
                switch(lastDirection) {
                    case 'up':
                        y -= 100;
                    break;
                    case 'down':
                        y += 100;
                    break;
                    case 'left':
                        x -= 100;
                    break;
                    case 'right':
                        x += 100;
                    break;
                    default:
                }
        }
    });
    // STOP 함수
    function stop() {
        dy = 0;
        dx = 0;
        char.style.left = '0px';
        char.style.top = '0px';
    }
moveChar();

</script>
<!-- Gun-->

<script>
// attack 함수 코드
    function attack() {
        const ground = document.getElementById('ground'); // ground 요소를 가져옴
        const gun = document.createElement('div'); //새로운 div 요소 생성
        gun.classList.add('gun'); // gun 클래스 추가
        // 캐릭터 위치에서 약간 이동하여 총을 배치    
        const charRect = char.getBoundingClientRect();
        gun.style.left = (charRect.left + charRect.width / 2 - 15) + 'px'; // 총의 가로 중앙에 배치
        gun.style.top = (charRect.top + charRect.height / 2 - 15) + 'px'; // 총의 세로 중앙에 배치
        ground.append(gun); // gun을 ground에 추가
        setTimeout(() => {
                gun.remove();
            }, 1000);

    }
// attack 함수 실행 코드
    window.addEventListener('keydown', function(event) {
        if (event.key === 'd' || event.key === 'D'){
            attack(); // 키보드 이벤트가 발생하면 attack 함수 호출
        }
        
    });
</script>

