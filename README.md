# cslt
<div style="padding: 10px; border: 1px solid #ccc; border-radius: 5px;">
  <div style="margin: 10px 0;">
    <label for="name">留言人：</label>
    <input type="text" id="name" placeholder="请输入你的昵称" style="padding: 5px; width: 200px;">
  </div>
  <div style="margin: 10px 0;">
    <label for="time">时间：</label>
    <input type="date" id="time" style="padding: 5px; width: 200px;">
  </div>
  <div style="margin: 10px 0;">
    <label for="content">留言内容：</label><br>
    <textarea id="content" rows="5" cols="30" placeholder="请输入留言内容" style="padding: 5px;"></textarea>
  </div>
  <button type="button" onclick="addMessage()" style="padding: 6px 12px; background: #424642; color: #fff; border: none; border-radius: 3px;">提交留言</button>
</div>
<script>
// 页面加载时自动渲染本地存储的留言
window.onload = function() {
    renderMessages();
}

// 渲染留言列表到页面
function renderMessages() {
    let messageList = document.getElementById("messageList");
    // 读取localStorage中的留言，无数据则初始化为空数组
    let messages = localStorage.getItem("messages");
    messages = messages ? JSON.parse(messages) : [];
    
    // 清空原有内容，避免重复渲染
    messageList.innerHTML = "";
    // 循环生成每条留言的DOM元素
    messages.forEach(msg => {
        let newMsg = document.createElement("div");
        newMsg.style.padding = "8px 0";
        newMsg.style.borderBottom = "1px dashed #ccc";
        newMsg.innerHTML = `<strong>留言人</strong>：${msg.name}&nbsp;&nbsp;&nbsp;<strong>时间</strong>：${msg.time}<br><strong>内容</strong>：${msg.content}`;
        messageList.appendChild(newMsg);
    });
}

// 添加新留言并持久化存储
function addMessage() {
    // 获取输入框的值
    let name = document.getElementById("name").value;
    let time = document.getElementById("time").value;
    let content = document.getElementById("content").value;
    
    // 验证输入完整性
    if (!name || !time || !content) {
        alert("请填写完整留言信息！");
        return;
    }
    
    // 读取并解析现有留言
    let messages = localStorage.getItem("messages");
    messages = messages ? JSON.parse(messages) : [];
    
    // 新增留言对象到数组
    messages.push({name, time, content});
    // 转为JSON字符串存入localStorage
    localStorage.setItem("messages", JSON.stringify(messages));
    
    // 重新渲染留言列表
    renderMessages();
    
    // 清空输入框
    document.getElementById("name").value = "";
    document.getElementById("time").value = "";
    document.getElementById("content").value = "";
    
    alert("留言提交成功！");
}
</script>

