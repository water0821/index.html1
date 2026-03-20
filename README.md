# index.html1
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>網紅時代的收入計算 - 專題報導</title>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        /* 基礎樣式設定 */
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: "Microsoft JhengHei", sans-serif; }
        body { background-color: #f8f9fa; color: #333; line-height: 1.8; }

        /* 導覽列 */
        nav {
            position: fixed; top: 0; width: 100%; background: rgba(255, 255, 255, 0.95);
            padding: 15px 50px; display: flex; justify-content: space-between;
            align-items: center; z-index: 1000; box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }
        .logo { font-weight: bold; font-size: 24px; color: #d9534f; }
        nav ul { display: flex; list-style: none; }
        nav ul li { margin-left: 30px; }
        nav ul li a { text-decoration: none; color: #333; font-weight: 500; transition: 0.3s; }
        nav ul li a:hover { color: #d9534f; }

        /* 全螢幕封面圖 */
        .hero {
            height: 100vh;
            background: linear-gradient(rgba(0,0,0,0.5), rgba(0,0,0,0.5)), 
                        url('https://images.unsplash.com/photo-1557804506-669a67965ba0?auto=format&fit=crop&w=1600&q=80');
            background-size: cover; background-position: center;
            display: flex; flex-direction: column; justify-content: center;
            align-items: center; color: white; text-align: center; padding: 20px;
        }
        .hero h1 { font-size: 3.5rem; margin-bottom: 20px; letter-spacing: 5px; }
        .hero p { font-size: 1.2rem; max-width: 700px; }

        /* 內文區塊 */
        .section { padding: 80px 10%; max-width: 1200px; margin: auto; }
        .section h2 { text-align: center; font-size: 2rem; margin-bottom: 40px; position: relative; }
        .section h2::after { content: ''; display: block; width: 50px; height: 3px; background: #d9534f; margin: 15px auto; }

        /* 卡片佈局 */
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 30px; }
        .card { background: white; border-radius: 10px; overflow: hidden; box-shadow: 0 5px 15px rgba(0,0,0,0.05); transition: 0.3s; }
        .card:hover { transform: translateY(-10px); }
        .card
