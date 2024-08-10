<script lang="ts">
  let editorContent: HTMLElement;

  function applyStyle(style: string, value: string = '') {
    document.execCommand(style, false, value);
  }

  function resetStyles() {
    // Clear HTML Tags
    if (editorContent) {
      // Get the plain text content of the element by using innerText or textContent
      const plainText = editorContent.innerText || editorContent.textContent;
      
      // Clear the element's content and set it to the plain text
      editorContent.innerHTML = plainText;
    }

    editorContent.style.textAlign = 'left';  // Reset alignment to default (left)
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

  .toolbar button, .toolbar select {
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
    <button on:click={() => editorContent.style.textAlign = 'left'}>Align Left</button>
    <button on:click={() => editorContent.style.textAlign = 'center'}>Align Center</button>
    <button on:click={() => editorContent.style.textAlign = 'right'}>Align Right</button>
    <button on:click={() => editorContent.style.textAlign = 'justify'}>Justify</button>
    <button on:click={resetStyles}>Reset Styles</button>
    <!-- <button on:click={getContent}>Get Content</button> -->
    <select on:change={(event) => applyStyle('fontSize', event.target.value)}>
      <option value="3">Font Size</option>
      <option value="1">10px</option>
      <option value="2">13px</option>
      <option value="3">16px</option>
      <option value="4">18px</option>
      <option value="5">24px</option>
      <option value="6">32px</option>
      <option value="7">48px</option>
    </select>
    <select on:change={(event) => applyStyle('fontName', event.target.value)}>
      <option value="Arial">Font Family</option>
      <option value="Arial">Arial</option>
      <option value="Courier New">Courier New</option>
      <option value="Georgia">Georgia</option>
      <option value="Times New Roman">Times New Roman</option>
      <option value="Verdana">Verdana</option>
    </select>
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
