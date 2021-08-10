<svelte:options tag="floating-chat" />

<script>
  import { get_current_component } from 'svelte/internal'
  const component = get_current_component()

	export let content
	export let height = '600px'
	export let width = '400px'
	export let textopen = 'Open'
	export let textclose = 'Close'
	export let textcolor = '#000000'
	export let background = '#FFFFFF'
	export let logo
	export let position = 'bottom right'
	export let font
	export let borderradius = '28px'
	export let opened = false

	const positiony = position.split(' ')[0] === 'top' ? 'initial': '0'
	const positionx = position.split(' ')[1] === 'left' ? 'initial': '0'

	export function open () {
		opened = true
    component?.dispatchEvent(new CustomEvent('open', { detail: null, composed: true }))
	}

	export function close () {
		opened = false
		component?.dispatchEvent(new CustomEvent('close', { detail: null, composed: true }))
	}

	export function toggle () {
		opened ? close() : open()
    component?.dispatchEvent(new CustomEvent('toggle', { detail: null, composed: true }))
	}
</script>

<main>
	<button
    title="{opened ? textclose: textopen}"
    aria-label="{opened ? textclose: textopen}"
		class="floating-chat {opened ? 'floating-chat-opened': 'floating-chat-closed'}"
		style="--height: {height}; --width: {width}; --textcolor: {textcolor}; --background: {background}; --borderradius: {borderradius}; --font: {font}; --logo: {logo}; --positiony: {positiony}; --positionx: {positionx}"
		on:click={toggle}
	>
		<div class="floating-chat-text">{opened ? textclose : textopen}</div>
		<div class="floating-chat-content">
			<iframe src="{content}" title="{textopen}" allow="microphone *"></iframe>
		</div>
	</button>
</main>

<style>
	.floating-chat {
    padding: 0;
    border: none;
    box-shadow: 0 1px 2px 0 rgba(60,64,67,0.302),0 1px 3px 1px rgba(60,64,67,0.149);
    font-family: var(--font), -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
    background: var(--background);
    border-radius: var(--borderradius);
    cursor: pointer;
    transition: all .45s cubic-bezier(.4, 0, .2, 1);
    position: fixed;
    bottom: var(--positiony);
    right: var(--positionx);
    margin: 16px;
    display: flex;
    flex-direction: column;
    z-index: 999
  }

  .floating-chat-text {
    min-width: 56px;
    color: var(--textcolor);
    display: inline-flex;
    align-items: center;
    font-weight: 500;
    padding: 0 24px 0 0;
    font-size: .875rem;
    height: 48px
  }

  .floating-chat-text:before {
    min-width: 56px;
    height: 48px;
    background-position: center;
    background-repeat: no-repeat;
    background-size: 24px;
    background-image: var(--logo);
    content: ''
  }

  .floating-chat:hover {
    box-shadow: 0 1px 3px 0 rgba(60,64,67,0.302), 0 4px 8px 3px rgba(60,64,67,0.149)
  }

  .floating-chat:not(.floating-chat-closed) {
    border-radius: calc(var(--borderradius) / 2)
  }

  .floating-chat:not(.floating-chat-closed) > .floating-chat-text:before {
    background: var(--textcolor);
    -webkit-mask: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>') no-repeat center;
    mask: url('data:image/svg+xml;utf8,<svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><line x1="18" y1="6" x2="6" y2="18"></line><line x1="6" y1="6" x2="18" y2="18"></line></svg>') no-repeat center
  }

  .floating-chat-content {
    display: flex;
		align-self: flex-end;
    height: var(--height);
    width: var(--width);
    transition: all .45s cubic-bezier(.4, 0, .2, 1);
    opacity: 1
  }

  .floating-chat:not(.floating-chat-closed) > .floating-chat-content {
    padding-bottom: 16px;
  }

	.floating-chat-content iframe {
		height: 100%;
		width: 100%;
		border: none;
	}

  .floating-chat-closed > .floating-chat-content {
    width: 0;
    height: 0;
    opacity: 0
  }

  @media screen and (max-width: 720px) {
    .floating-chat:not(.floating-chat-closed) {
      margin: 0px;
      border-radius: 0px
    }

    .floating-chat:not(.floating-chat-closed) > .floating-chat-content {
      width: 100vw;
      max-height: 100vh;
      height: calc(100vh - 56px);
      padding-bottom: 0px
    }

    .floating-chat-text {
      padding: 0;
      height: 56px;
      font-size: 0;
    }
  }
</style>
