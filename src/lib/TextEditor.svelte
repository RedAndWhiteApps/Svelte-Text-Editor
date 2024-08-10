<script lang="ts">
    let editorContent: HTMLElement;
  
    function format(command: string, value?: string) {
      document.execCommand(command, false, value);
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
      <button on:click={() => format('bold')}>Bold</button>
      <button on:click={() => format('italic')}>Italic</button>
      <button on:click={() => format('underline')}>Underline</button>
      <button on:click={() => format('insertOrderedList')}>Ordered List</button>
      <button on:click={() => format('insertUnorderedList')}>Unordered List</button>
      <button on:click={() => format('createLink', prompt('Enter a URL:', 'http://'))}>Link</button>
      <button on:click={getContent}>Get Content</button>
    </div>
  
    <div class="editor-container">
      <div
        class="editor"
        contenteditable="true"
        bind:this={editorContent}
        placeholder="Type here..."
      ></div>
    </div>
  </div>
  