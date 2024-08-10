<script lang="ts">
  let editorContent: HTMLElement;

  function applyStyle(style: string) {
    document.execCommand(style, false);
  }

  function resetStyles() {
    clearHTMLTags(editorContent);
    editorContent.style.textAlign = 'left';  // Reset alignment to default (left)
  }

  function clearHTMLTags(editorContent) {
    if (editorContent) {
      // Get the plain text content of the element by using innerText or textContent
      const plainText = editorContent.innerText || editorContent.textContent;
      
      // Clear the element's content and set it to the plain text
      editorContent.innerHTML = plainText;
    }
  }

  function getContent() {
    alert(editorContent.innerHTML);
  }
</script>



<style>
  .editor-container {
    border: 1px solid #ccc;
    padding: 10px;
    margin-bottom: 10px;
  }

  .toolbar button {
    margin-right: 5px;
  }

  .editor {
    min-height: 200px;
    outline: none;
  }
</style>

<div>
  <div class="toolbar">
    <button on:click={() => applyStyle('bold')}>Bold</button>
    <button on:click={() => applyStyle('italic')}>Italic</button>
    <button on:click={() => applyStyle('underline')}>Underline</button>
    <button on:click={() => applyStyle('insertOrderedList')}>Ordered List</button>
    <button on:click={() => applyStyle('insertUnorderedList')}>Unordered List</button>
    <button on:click={() => applyStyle('createLink', prompt('Enter a URL:', 'http://'))}>Link</button>
    <button on:click={() => editorContent.style.textAlign = 'left'}>Align Left</button>
    <button on:click={() => editorContent.style.textAlign = 'center'}>Align Center</button>
    <button on:click={() => editorContent.style.textAlign = 'right'}>Align Right</button>
    <button on:click={resetStyles}>Reset Styles</button>
    <button on:click={getContent}>Get Content</button>
  </div>

  <div class="editor-container">
    <p
      class="editor"
      contenteditable="true"
      bind:this={editorContent}
      placeholder="Type here..."
    ></p>
  </div>
</div>
