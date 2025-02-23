<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <title>소셜 미디어 커뮤니티</title>
    <style>
        body { font-family: -apple-system, Arial, sans-serif; margin: 0; padding: 20px; background: #f0f2f5; line-height: 1.5; }
        .container { max-width: 700px; margin: 0 auto; }
        h1 { text-align: center; color: #1c1e21; font-size: 24px; margin-bottom: 20px; }
        
        /* 인증 섹션 */
        .auth-section { background: white; padding: 15px; border-radius: 10px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); margin-bottom: 20px; display: flex; gap: 10px; align-items: center; }
        .auth-section input { padding: 8px; border: 1px solid #ddd; border-radius: 5px; }
        .auth-section button { padding: 8px 16px; background: #1b74e4; color: white; border: none; border-radius: 6px; }
        .auth-section button:hover { background: #165fc7; }
        .auth-status { font-size: 14px; color: #65676b; }

        /* 게시물 작성 폼 */
        .post-form { background: white; padding: 15px; border-radius: 10px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); margin-bottom: 20px; display: none; }
        .post-form textarea { width: 100%; padding: 12px; border: none; border-radius: 8px; background: #f5f6f5; font-size: 16px; resize: none; height: 60px; box-sizing: border-box; }
        .post-form input[type="file"] { margin: 10px 0; }
        .post-form button { width: 100%; margin-top: 10px; padding: 10px; background: #1b74e4; color: white; border: none; border-radius: 6px; font-size: 14px; cursor: pointer; }
        .post-form button:hover { background: #165fc7; }

        /* 게시물 목록 */
        .posts { display: flex; flex-direction: column; gap: 15px; }
        .post { background: white; padding: 15px; border-radius: 10px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .post-header { display: flex; align-items: center; gap: 10px; margin-bottom: 10px; }
        .post-header img { width: 36px; height: 36px; border-radius: 50%; object-fit: cover; }
        .post-header .user-info { flex: 1; }
        .post-header strong { color: #1c1e21; font-size: 14px; }
        .post-header small { color: #65676b; font-size: 12px; }
        .post-content { margin: 10px 0; font-size: 15px; color: #1c1e21; word-wrap: break-word; }
        .post-image { max-width: 100%; border-radius: 8px; margin-top: 10px; }

        /* 게시물 액션 버튼 */
        .post-actions { display: flex; gap: 20px; padding: 10px 0; border-top: 1px solid #ebedf0; }
        .post-actions button { background: none; color: #65676b; font-size: 13px; padding: 5px 10px; border: none; cursor: pointer; }
        .post-actions button:hover { color: #1b74e4; }
        .post-actions button.liked { color: #1b74e4; font-weight: bold; }

        /* 댓글 섹션 */
        .comments { padding: 10px 0; border-top: 1px solid #ebedf0; }
        .comment { display: flex; gap: 10px; margin: 8px 0; }
        .comment img { width: 32px; height: 32px; border-radius: 50%; }
        .comment .comment-text { background: #f0f2f5; padding: 8px 12px; border-radius: 15px; font-size: 13px; color: #1c1e21; }
        .comment small { color: #65676b; font-size: 11px; margin-top: 2px; }
        .comment-input { display: flex; gap: 10px; margin-top: 10px; }
        .comment-input input { flex: 1; padding: 8px 12px; border: none; border-radius: 20px; background: #f0f2f5; font-size: 13px; }
        .comment-input button { padding: 8px 16px; background: #e4e6eb; color: #1c1e21; border: none; border-radius: 20px; font-size: 13px; }
        .comment-input button:hover { background: #d8dade; }

        /* 단축키 안내 */
        .shortcuts { margin-top: 20px; padding: 15px; background: white; border-radius: 10px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .shortcuts h3 { margin: 0 0 10px; font-size: 16px; color: #1c1e21; }
        .shortcuts ul { list-style: none; padding: 0; font-size: 14px; color: #65676b; }
        .shortcuts li { margin: 5px 0; }
    </style>
</head>
<body>
    <div class="container">
        <h1>소셜 미디어 커뮤니티</h1>
        <div class="auth-section" id="authSection">
            <input type="text" id="username" placeholder="사용자 이름">
            <button onclick="login()">로그인</button>
            <button onclick="logout()" style="display: none;" id="logoutBtn">로그아웃</button>
            <span class="auth-status" id="authStatus">로그인하지 않음</span>
        </div>
        <div class="post-form" id="postForm">
            <textarea id="postInput" placeholder="지금 무슨 생각을 하고 계신가요?"></textarea>
            <input type="file" id="imageInput" accept="image/*">
            <button onclick="addPost()">게시 (P)</button>
        </div>
        <div class="posts" id="postList"></div>
        <div class="shortcuts">
            <h3>키보드 단축키 안내</h3>
            <ul>
                <li><strong>P</strong>: 새 게시물 추가</li>
                <li><strong>E</strong>: 선택한 게시물 수정</li>
                <li><strong>D</strong>: 선택한 게시물 삭제</li>
                <li><strong>L</strong>: 선택한 게시물 좋아요</li>
                <li><strong>C</strong>: 선택한 게시물에 댓글 추가 (입력란 포커스)</li>
                <li><strong>Ctrl+S</strong>: 게시물 목록 저장 (콘솔 출력)</li>
            </ul>
            <h3>댓글 기능 상세 설명</h3>
            <ul>
                <li>각 게시물 하단에 댓글 입력란과 버튼이 표시됩니다.</li>
                <li>댓글 입력 후 "댓글" 버튼 클릭 또는 Enter 키로 추가됩니다.</li>
                <li><strong>C</strong> 단축키로 댓글 입력란에 포커스 이동.</li>
                <li>댓글은 최대 200자로 제한되며, 작성 시간과 함께 표시됩니다.</li>
                <li>로그인한 사용자의 이름이 댓글에 반영됩니다.</li>
            </ul>
        </div>
    </div>

    <script>
        let posts = [];
        let selectedPostId = null;
        let currentUser = null;

        // 사용자 인증: 로그인
        function login() {
            const username = document.getElementById('username').value.trim();
            if (username) {
                currentUser = { username, profileImg: 'https://via.placeholder.com/36' }; // 실제 프로필 이미지 URL로 대체 가능
                document.getElementById('authStatus').textContent = `${username}님 로그인`;
                document.getElementById('username').style.display = 'none';
                document.getElementById('logoutBtn').style.display = 'inline';
                document.getElementById('postForm').style.display = 'block';
                document.querySelector('#authSection button[onclick="login()"]').style.display = 'none';
            }
        }

        // 사용자 인증: 로그아웃
        function logout() {
            currentUser = null;
            document.getElementById('authStatus').textContent = '로그인하지 않음';
            document.getElementById('username').style.display = 'inline';
            document.getElementById('username').value = '';
            document.getElementById('logoutBtn').style.display = 'none';
            document.getElementById('postForm').style.display = 'none';
            document.querySelector('#authSection button[onclick="login()"]').style.display = 'inline';
        }

        // 게시물 추가
        function addPost() {
            if (!currentUser) return alert('로그인하세요.');
            const content = document.getElementById('postInput').value.trim();
            const fileInput = document.getElementById('imageInput');
            const file = fileInput.files[0];
            let imageUrl = '';

            if (file) {
                const reader = new FileReader();
                reader.onload = e => {
                    imageUrl = e.target.result;
                    createPost(content, imageUrl);
                    fileInput.value = '';
                };
                reader.readAsDataURL(file);
            } else {
                createPost(content, imageUrl);
            }
        }

        function createPost(content, imageUrl) {
            if (content || imageUrl) {
                posts.unshift({
                    id: Date.now(),
                    content: content,
                    image: imageUrl,
                    username: currentUser.username,
                    profileImg: currentUser.profileImg,
                    date: new Date().toLocaleString('ko-KR'),
                    likes: 0,
                    liked: false,
                    comments: []
                });
                document.getElementById('postInput').value = '';
                renderPosts();
            }
        }

        // 게시물 수정
        function editPost(id) {
            const post = posts.find(p => p.id === id);
            if (post.username !== currentUser.username) return alert('본인의 게시물만 수정 가능합니다.');
            const newContent = prompt("게시물 수정:", post.content);
            if (newContent !== null && newContent.trim()) {
                post.content = newContent.trim();
                renderPosts();
            }
        }

        // 게시물 삭제
        function deletePost(id) {
            const post = posts.find(p => p.id === id);
            if (post.username !== currentUser.username) return alert('본인의 게시물만 삭제 가능합니다.');
            if (confirm("이 게시물을 삭제하시겠습니까?")) {
                posts = posts.filter(p => p.id !== id);
                renderPosts();
            }
        }

        // 좋아요 토글
        function toggleLike(id) {
            const post = posts.find(p => p.id === id);
            post.liked = !post.liked;
            post.likes += post.liked ? 1 : -1;
            renderPosts();
        }

        // 댓글 추가
        function addComment(id) {
            if (!currentUser) return alert('로그인하세요.');
            const post = posts.find(p => p.id === id);
            const commentInput = document.getElementById(`comment-${id}`).value.trim();
            if (commentInput) {
                post.comments.push({
                    text: commentInput.substring(0, 200),
                    username: currentUser.username,
                    profileImg: currentUser.profileImg,
                    date: new Date().toLocaleString('ko-KR')
                });
                document.getElementById(`comment-${id}`).value = '';
                renderPosts();
            }
        }

        // 게시물 렌더링
        function renderPosts() {
            const postList = document.getElementById('postList');
            postList.innerHTML = '';
            posts.forEach(post => {
                const div = document.createElement('div');
                div.className = 'post';
                div.innerHTML = `
                    <div class="post-header">
                        <img src="${post.profileImg}" alt="프로필">
                        <div class="user-info">
                            <strong>${post.username}</strong>
                            <small>${post.date}</small>
                        </div>
                    </div>
                    <div class="post-content">${post.content}</div>
                    ${post.image ? `<img src="${post.image}" class="post-image" alt="첨부 이미지">` : ''}
                    <div class="post-actions">
                        <button onclick="toggleLike(${post.id})" class="${post.liked ? 'liked' : ''}">
                            좋아요 (${post.likes})
                        </button>
                        ${currentUser && post.username === currentUser.username ? `
                            <button onclick="editPost(${post.id})">수정 (E)</button>
                            <button onclick="deletePost(${post.id})">삭제 (D)</button>
                        ` : ''}
                    </div>
                    <div class="comments">
                        ${post.comments.map(c => `
                            <div class="comment">
                                <img src="${c.profileImg}" alt="프로필">
                                <div>
                                    <div class="comment-text"><strong>${c.username}</strong> ${c.text}</div>
                                    <small>${c.date}</small>
                                </div>
                            </div>
                        `).join('')}
                        <div class="comment-input">
                            <input type="text" id="comment-${post.id}" placeholder="댓글을 입력하세요..." onkeydown="if(event.key === 'Enter') addComment(${post.id})">
                            <button onclick="addComment(${post.id})">댓글 (C)</button>
                        </div>
                    </div>
                `;
                postList.appendChild(div);
            });
        }

        // 게시물 저장
        function savePosts(event) {
            if (event) event.preventDefault();
            console.log('게시물 목록:', posts);
            alert('게시물이 콘솔에 저장되었습니다.');
        }

        // 키보드 단축키 설정
        document.addEventListener('keydown', e => {
            if (e.target.tagName === 'TEXTAREA' || e.target.tagName === 'INPUT') {
                if (e.ctrlKey && e.key === 's') savePosts(e);
                return;
            }
            switch (e.key.toLowerCase()) {
                case 'p': addPost(); break;
                case 'e': if (selectedPostId) editPost(selectedPostId); break;
                case 'd': if (selectedPostId) deletePost(selectedPostId); break;
                case 'l': if (selectedPostId) toggleLike(selectedPostId); break;
                case 'c': if (selectedPostId) document.getElementById(`comment-${selectedPostId}`).focus(); break;
                case 's': if (e.ctrlKey) savePosts(e); break;
            }
        });

        // 게시물 선택
        document.getElementById('postList').addEventListener('click', e => {
            const postDiv = e.target.closest('.post');
            if (postDiv) selectedPostId = Number(postDiv.querySelector('.post-actions button').getAttribute('onclick').match(/\d+/)[0]);
        });
    </script>
</body>
</html>
