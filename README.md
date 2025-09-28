<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>イベント登録</title>
    <style>
        /* 基本スタイル */
        body { font-family: Arial, sans-serif; }
        #register-btn { cursor: pointer; border: 1px solid #ccc; padding: 10px; border-radius: 5px; }
        
        /* モーダルスタイル */
        .modal { display: none; position: fixed; z-index: 1000; left: 0; top: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.5); }
        .modal-content { background-color: white; margin: 15% auto; padding: 20px; border: 1px solid #888; width: 80%; max-width: 400px; border-radius: 10px; }
        .close { color: #aaa; float: right; font-size: 28px; font-weight: bold; cursor: pointer; }
        .close:hover { color: black; }
        
        /* 選択ボタン */
        .status-btn { display: block; width: 100%; margin: 10px 0; padding: 10px; background: #f0f0f0; border: none; cursor: pointer; border-radius: 5px; }
        .status-btn:hover { background: #ddd; }
        
        /* フォーム */
        .form-group { margin: 10px 0; }
        .form-group label { display: block; margin-bottom: 5px; }
        .form-group input { width: 100%; padding: 8px; border: 1px solid #ccc; border-radius: 3px; }
        
        /* 送信ボタン */
        #submit-btn { background: #4CAF50; color: white; padding: 10px; border: none; width: 100%; border-radius: 5px; cursor: pointer; }
        #submit-btn:hover { background: #45a049; }
    </style>
</head>
<body>

    <!-- 画像ボタン（例: 実際の画像URLに置き換え） -->
    <img id="register-btn" src="https://via.placeholder.com/200x50?text=登録ボタン](https://www.dropbox.com/scl/fi/sm7zxk4d4s1kie8bisei2/.jpg?rlkey=ib8ng6v41acc4094oejikz7u5&st=pae7cw45&dl=1" alt="登録ボタン">

    <!-- ステータス選択モーダル -->
    <div id="status-modal" class="modal">
        <div class="modal-content">
            <span class="close">&times;</span>
            <h2>参加状況を選択</h2>
            <button class="status-btn" onclick="showForm('参加')">参加</button>
            <button class="status-btn" onclick="showForm('不参加')">不参加</button>
            <button class="status-btn" onclick="showForm('未定')">未定</button>
        </div>
    </div>

    <!-- 入力フォームモーダル -->
    <div id="form-modal" class="modal">
        <div class="modal-content">
            <span class="close" onclick="closeForm()">&times;</span>
            <h2 id="form-title">登録情報入力</h2>
            <form id="register-form">
                <div class="form-group">
                    <label for="name">名前:</label>
                    <input type="text" id="name" required>
                </div>
                <div class="form-group">
                    <label for="email">メールアドレス:</label>
                    <input type="email" id="email" required>
                </div>
                <div class="form-group">
                    <label for="phone">電話番号:</label>
                    <input type="tel" id="phone">
                </div>
                <div class="form-group">
                    <label for="line">LINE名:</label>
                    <input type="text" id="line">
                </div>
                <input type="hidden" id="status">
                <button type="submit" id="submit-btn">送信</button>
            </form>
        </div>
    </div>

    <script>
        // モーダル表示/非表示関数
        function openStatusModal() {
            document.getElementById('status-modal').style.display = 'block';
        }
        function closeStatusModal() {
            document.getElementById('status-modal').style.display = 'none';
        }
        function showForm(status) {
            document.getElementById('status').value = status;
            document.getElementById('form-title').textContent = status + 'の登録情報入力';
            document.getElementById('status-modal').style.display = 'none';
            document.getElementById('form-modal').style.display = 'block';
        }
        function closeForm() {
            document.getElementById('form-modal').style.display = 'none';
            document.getElementById('register-form').reset();
        }

        // 画像ボタンクリックイベント
        document.getElementById('register-btn').addEventListener('click', openStatusModal);

        // モーダルクローズイベント
        window.onclick = function(event) {
            const statusModal = document.getElementById('status-modal');
            const formModal = document.getElementById('form-modal');
            if (event.target == statusModal) closeStatusModal();
            if (event.target == formModal) closeForm();
        }

        // フォーム送信イベント（mailtoでメール送信。集計にはGoogle Sheets連携推奨）
        document.getElementById('register-form').addEventListener('submit', function(e) {
            e.preventDefault();
            const status = document.getElementById('status').value;
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const phone = document.getElementById('phone').value;
            const line = document.getElementById('line').value;

            // メール本文の生成（集計用）
            const subject = `イベント登録: ${status} - ${name}`;
            const body = `参加状況: ${status}\n名前: ${name}\nメール: ${email}\n電話: ${phone}\nLINE: ${line}\n\n--- 集計用データ ---`;

            // 2つのメールアドレスに送信（クライアントサイドなので、メールクライアントが開く）
            const mailto1 = `mailto:zznjip@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
            const mailto2 = `mailto:mermaidnomail1125@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
            
            // 両方に送信（ブラウザの制限で複数同時は難しいので、順次開くか、1つにCC）
            window.location.href = `mailto:zznjip@gmail.com,mermaidnomail1125@gmail.com?subject=${encodeURIComponent(subject)}&body=${encodeURIComponent(body)}`;
            
            alert('登録が完了しました！メールクライアントが開きます。');
            closeForm();
        });
    </script>

</body>
</html>
