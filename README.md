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
