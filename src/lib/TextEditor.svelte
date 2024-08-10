<script lang="ts">
	let editorContent: HTMLElement;

	function applyStyle(style: string, value: string = '') {
		document.execCommand(style, false, value);
		updateActiveButtons(); // Update button styles immediately
	}

	function resetStyles() {
		// Clear HTML Tags
		if (editorContent) {
			// Get the plain text content of the element by using innerText or textContent
			const plainText = editorContent.innerText || editorContent.textContent;

			// Clear the element's content and set it to the plain text
			editorContent.innerHTML = plainText;
		}

		editorContent.style.textAlign = 'left'; // Reset alignment to default (left)
		updateActiveButtons(); // Update button styles immediately
	}

	function getContent() {
		alert(editorContent.innerHTML);
	}

	function updateActiveButtons() {
		const commands = ['bold', 'italic', 'underline'];
		commands.forEach((command) => {
			const button = document.querySelector(`button[data-command=${command}]`);
			if (document.queryCommandState(command)) {
				button.classList.add('active');
			} else {
				button.classList.remove('active');
			}
		});

		// Text alignment
		const alignments = ['left', 'center', 'right', 'justify'];
		alignments.forEach((align) => {
			const button = document.querySelector(`button[data-align=${align}]`);
			if (editorContent.style.textAlign === align) {
				button.classList.add('active');
			} else {
				button.classList.remove('active');
			}
		});
	}

	function applyAlignment(alignment: string) {
		editorContent.style.textAlign = alignment;
		updateActiveButtons(); // Update button styles immediately
	}

	document.addEventListener('selectionchange', updateActiveButtons); // Listen to cursor movements
</script>

<!-- Include FontAwesome or your preferred icon library -->
<link
	rel="stylesheet"
	href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css"
/>

<div>
	<div class="toolbar">
		<button data-command="bold" on:click={() => applyStyle('bold')}>
			<i class="fas fa-bold"></i>
		</button>
		<button data-command="italic" on:click={() => applyStyle('italic')}>
			<i class="fas fa-italic"></i>
		</button>
		<button data-command="underline" on:click={() => applyStyle('underline')}>
			<i class="fas fa-underline"></i>
		</button>
		<button on:click={() => applyStyle('insertOrderedList')}>
			<i class="fas fa-list-ol"></i>
		</button>
		<button on:click={() => applyStyle('insertUnorderedList')}>
			<i class="fas fa-list-ul"></i>
		</button>
		<button data-align="left" on:click={() => applyAlignment('left')}>
			<i class="fas fa-align-left"></i>
		</button>
		<button data-align="center" on:click={() => applyAlignment('center')}>
			<i class="fas fa-align-center"></i>
		</button>
		<button data-align="right" on:click={() => applyAlignment('right')}>
			<i class="fas fa-align-right"></i>
		</button>
		<button data-align="justify" on:click={() => applyAlignment('justify')}>
			<i class="fas fa-align-justify"></i>
		</button>
		<button on:click={resetStyles}>
			<i class="fas fa-undo"></i>
		</button>
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

<style>
	.editor-container {
		border: 1px solid #ccc;
		padding: 10px;
		margin-bottom: 10px;
	}

	.toolbar button,
	.toolbar select {
		margin-right: 5px;
		background: none;
		border: none;
		cursor: pointer;
		font-size: 16px;
	}

	.toolbar button i {
		font-size: 18px;
		color: #333;
	}

	.toolbar button:hover i {
		color: #000;
	}

	.toolbar button.active i {
		color: #007bff; /* Change color for active buttons */
	}

	.editor {
		min-height: 200px;
		outline: none;
	}

	.toolbar select {
		padding: 2px;
		font-size: 16px;
	}
</style>
