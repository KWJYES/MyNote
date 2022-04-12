# 复制文本到剪切板

```java
/**
 * 复制文案到剪切板
 * @param sentence
 */
private void copySentence(String sentence){
    ClipboardManager clipboardManager = (ClipboardManager) parent.getContext()
            .getSystemService(Context.CLIPBOARD_SERVICE);
    ClipData clip = ClipData.newPlainText("精美文案", sentence);
    clipboardManager.setPrimaryClip(clip);//放到剪切板
}
```