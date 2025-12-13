# cslt
<div style="width: 80%; max-width: 1000px; margin: 20px auto; padding: 25px; background: #f8f9fa; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.1);">
  <h2 style="color: #333; text-align: center; margin-bottom: 20px;">TinyWebDB 云端留言板</h2>
  
  <!-- 留言输入区 -->
  <div style="margin-bottom: 25px;">
    <div style="margin-bottom: 10px;">
      <label style="display: block; margin-bottom: 5px; color: #555;">昵称</label>
      <input type="text" id="nameInput" placeholder="请输入你的昵称" style="width: 100%; padding: 10px; border: 1px solid #ddd; border-radius: 4px; box-sizing: border-box;">
    </div>
    <div>
      <label style="display: block; margin-bottom: 5px; color: #555;">留言内容</label>
      <textarea id="msgInput" rows="4" placeholder="写下想分享的内容..." style="width: 100%; padding: 10px; border: 1px solid #ddd; border-radius: 4px; resize: none; box-sizing: border-box;"></textarea>
    </div>
    <button onclick="submitMsg()" style="margin-top: 10px; padding: 10px 20px; background: #28a745; color: #fff; border: none; border-radius: 4px; cursor: pointer;">提交留言</button>
  </div>

  <!-- 留言展示区 -->
  <div id="msgList" style="border-top: 1px solid #eee; padding-top: 20px;">
    <p style="color: #999; text-align: center;">加载留言中...</p >
  </div>
</div>

<script>
// 公共 TinyWebDB 接口地址（无需注册，直接使用）
const TINY_WEBDB_URL = "https://tinywebdb.appinventor.mit.edu/api";
// 自定义存储键名（避免和他人数据冲突）
const STORAGE_KEY = "my_markdown_board_msgs_12345";

// 页面加载时获取云端留言
window.onload = loadMessages();

// 提交留言到云端
async function submitMsg() {
  const name = document.getElementById('nameInput').value.trim();
  const content = document.getElementById('msgInput').value.trim();
  if (!name || !content) {
    alert('昵称和留言不能为空！');
    return;
  }

  // 构造留言数据
  const msgData = {
    name: name,
    content: content,
    time: new Date().toLocaleString()
  };

  try {
    // 1. 先获取云端已有留言
    let oldMsgs = await getCloudData();
    // 2. 新增留言放到最前面
    oldMsgs.unshift(msgData);
    // 3. 保存更新后的数据到云端
    await saveCloudData(oldMsgs);
    // 4. 刷新留言列表
    loadMessages();
    // 5. 清空输入框
    document.getElementById('nameInput').value = '';
    document.getElementById('msgInput').value = '';
    alert('留言提交成功！');
  } catch (err) {
    alert('提交失败：' + err.message);
  }
}

// 从云端获取留言数据
async function getCloudData() {
  const response = await fetch(`${TINY_WEBDB_URL}/getvalue?tag=${STORAGE_KEY}`);
  const data = await response.json();
  // 无数据时返回空数组
  return data.value ? JSON.parse(data.value) : [];
}

// 保存数据到云端
async function saveCloudData(msgArray) {
  const formData = new FormData();
  formData.append('tag', STORAGE_KEY);
  formData.append('value', JSON.stringify(msgArray));
  await fetch(`${TINY_WEBDB_URL}/storevalue`, {
    method: 'POST',
    body: formData
  });
}

// 渲染留言列表
async function loadMessages() {
  const list = document.getElementById('msgList');
  const messages = await getCloudData();

  if (messages.length === 0) {
    list.innerHTML = '<p style="color: #999; text-align: center;">暂无留言，快来抢占沙发~</p >';
    return;
  }

  let html = '';
  messages.forEach(item => {
    html += `
      <div style="padding: 15px; background: #fff; border-radius: 4px; margin-bottom: 10px; box-shadow: 0 1px 3px rgba(0,0,0,0.05);">
        <div style="display: flex; justify-content: space-between; margin-bottom: 8px;">
          <span style="font-weight: bold; color: #28a745;">${item.name}</span>
          <span style="font-size: 12px; color: #999;">${item.time}</span>
        </div>
        <p style="color: #333; margin: 0; line-height: 1.5;">${item.content}</p >
      </div>
    `;
  });
  list.innerHTML = html;
}
</script>
