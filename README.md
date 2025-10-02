<script>
(function() {
  // Bubble hiển thị khi bôi chọn
  var bubble = document.createElement("div");
  bubble.className = "highlight-popup";
  bubble.style.display = "none";
  bubble.style.position = "absolute";
  bubble.style.background = "#fff";
  bubble.style.border = "1px solid #ccc";
  bubble.style.padding = "4px";
  bubble.style.borderRadius = "6px";
  bubble.style.boxShadow = "0 2px 6px rgba(0,0,0,0.2)";
  document.body.appendChild(bubble);

  // Nút highlight (màu xanh lục)
  var highlightBtn = document.createElement("button");
  highlightBtn.innerHTML = "🌿";
  highlightBtn.title = "Highlight xanh lục";
  bubble.appendChild(highlightBtn);

  // Nút copy
  var copyBtn = document.createElement("button");
  copyBtn.innerHTML = "📋";
  copyBtn.title = "Copy";
  bubble.appendChild(copyBtn);

  // Nút xóa highlight
  var deleteBtn = document.createElement("button");
  deleteBtn.innerHTML = "❌";
  deleteBtn.title = "Xóa highlight";
  bubble.appendChild(deleteBtn);

  // Hàm lấy selection hiện tại
  function getSelectionText() {
    var sel = window.getSelection();
    return sel.rangeCount ? sel.getRangeAt(0) : null;
  }

  // Highlight xanh lục
  highlightBtn.addEventListener("click", function() {
    var range = getSelectionText();
    if (range && !range.collapsed) {
      var span = document.createElement("span");
      span.style.backgroundColor = "#66cc66";
      span.style.padding = "2px";
      span.appendChild(range.extractContents());
      range.insertNode(span);
      window.getSelection().removeAllRanges();
      bubble.style.display = "none";
    }
  });

  // Copy
  copyBtn.addEventListener("click", function() {
    var text = window.getSelection().toString();
    if (text) {
      navigator.clipboard.writeText(text);
      alert("Đã copy: " + text);
    }
    bubble.style.display = "none";
  });

  // Delete highlight
  deleteBtn.addEventListener("click", function() {
    var sel = window.getSelection();
    if (sel.rangeCount) {
      var range = sel.getRangeAt(0);
      var node = range.startContainer.parentNode;
      if (node.tagName === "SPAN" && node.style.backgroundColor === "rgb(102, 204, 102)") {
        node.outerHTML = node.innerHTML; // gỡ bỏ span
      }
    }
    bubble.style.display = "none";
  });

  // Hiện bubble khi bôi chữ
  document.onmouseup = function(e) {
    var text = window.getSelection().toString();
    if (text.length > 0) {
      var rect = window.getSelection().getRangeAt(0).getBoundingClientRect();
      bubble.style.top = (rect.top + window.scrollY - bubble.offsetHeight - 5) + "px";
      bubble.style.left = (rect.left + window.scrollX) + "px";
      bubble.style.display = "block";
    } else {
      bubble.style.display = "none";
    }
  };

})();
</script>
