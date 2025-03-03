Put "const minDelay = 60;
const maxDelay = 60;

const keyOverrides = {
[String.fromCharCode(160)]: ' ' // convert hardspace to normal space
};

function getTargetCharacters() {
const els = Array.from(document.querySelectorAll('.token span.token_unit'));
const chrs = els
.map(el => {
if (el.firstChild?.classList?.contains('_enter')) {
return '\n'; // special case: ENTER
}
let text = el.textContent[0];
return text;
})
.map(c => keyOverrides.hasOwnProperty(c) ? keyOverrides[c] : c); // convert special characters
return chrs;
}

function recordKey(chr) {
// send it straight to the internal API
window.core.record_keydown_time(chr);
}

function sleep(ms) {
return new Promise(r => setTimeout(r, ms));
}

async function autoPlay() {
const chrs = getTargetCharacters();
for (let i = 0; i < chrs.length; ++i) {
const c = chrs[i];
recordKey(c);
await sleep(Math.random() * (maxDelay - minDelay) + minDelay);
}

// Click the "Continue" button after finishing typing
await sleep(500); // Short pause before clicking continue
const continueButton = document.querySelector('button[aria-label="Continue"]');
if (continueButton) {
    continueButton.click(); 
}

// Start typing for the next level
await sleep(1000); 
autoPlay();
}

// Start the autoplay
autoPlay();" in the console using Ctr + Shift + c.
