<div id="dragonviewer" width="256" height="256" ></div>
<input id="blk" type="number" style="display:none" style="text-align:right" />

<script id="preview" mint="MINT_INSCRIPTION_ID">
// Retrieve render area
const root = document.getElementById('dragonviewer')
root.parentElement.style.width = '100%'
root.parentElement.style.height = '100%'
root.parentElement.style.padding = '0px'
root.parentElement.style.margin = '0px'

const orgWidth = 212
const orgHeight = 212
let scaleW = 1
let scaleH = 1
let blockNumber = '0'

// Define the color map with the given colors
const colorMap = {
    1: "#6D2BF8", // Purple
    2: "#AF89FE", // Lilac
    3: "#FDF64D", // Yellow
    4: "#2067F0", // Blue
    5: "#976F53", // Brown
    6: "#CBC7E3", // Light Grey
    7: "#15D96F", // Green
    8: "#FF64C1", // Pink
    9: "#F95E3C", // Orange
    0: "#585663", // Grey
}

// Define the color map with the given colors
const colorMap2 = {
    1: "#5922CD", // Purple
    2: "#9C6EFE", // Lilac
    3: "#FDE14D", // Yellow
    4: "#1C54C0", // Blue
    5: "#725540", // Brown
    6: "#8F8DA5", // Light Grey
    7: "#17B35F", // Green
    8: "#F343AC", // Pink
    9: "#EC5331", // Orange
    0: "#403F4A", // Dark Grey
}

// Define the color map with the given colors
const colorMap3 = {
    1: "#A2FF00", // GITD
    2: "#49EFEF", // Alien
    3: "#FFB800", // Gold
    4: "#FFA1FB", // Bubblegum
    5: "#FF7528", // Orange
    6: "#FF1E39", // Red
    7: "#00B127", // Green
    8: "#2A32FF", // Blue
    9: "#A9A8D6", // Platinum
}

// Define the color map with the given colors
const colorMap4 = {
    1: "#110726", // Purple
    2: "#1c142e", // Lilac
    3: "#26220b", // Yellow
    4: "#061126", // Blue
    5: "#261c15", // Brown
    6: "#212126", // Light Grey
    7: "#052615", // Green
    8: "#260b1b", // Pink
    9: "#260e08", // Orange
    0: "#212126", // Dark Grey
}
// Resize window
window.addEventListener('resize', resize, true)
resize()

// Retrieve content inscription id
let mintText = document.getElementById('preview').getAttribute('mint')

// Check no mint provided
if(mintText.includes('MINT_INSCRIPTION_ID')) {
  let input = document.getElementById('blk')
  input.style.display = 'block'
  input.style.position = 'absolute'
  input.style.fontSize = '20px'
  input.style.margin = '20px'
  input.style.top = '0'
   input.style.right = '0'
  input.value = blockNumber
  input.addEventListener('input',(event) => {
      blockNumber = input.value
      update()
  })
  update()
}
// Mint was provided
else {
  const request = new XMLHttpRequest()
  try {
    request.open('GET', '/content/' + mintText)
    request.responseType = 'text'
    request.addEventListener('load', () => initialize(request.response))
    request.addEventListener('error', () => console.error('XHR error'))
    request.send()
  } catch (error) {
    console.error(`XHR error ${request.status}`)
  }
}

function initialize(result) {
  if(result) {
    console.log('Result', result)
    data = JSON.parse(result)
    blockNumber = data.blk
  }
  update()
}

function resize(event) {
  root.width = window.innerWidth
  root.height = window.innerHeight
  scaleW = root.width / orgWidth
  scaleH = root.height / orgHeight
  offsetX = 0
  offsetY = 0
  if(scaleH < scaleW) {
    scaleW = scaleH
    offsetX = (root.width - orgWidth * scaleW) / 2
  }
  else {
    scaleH = scaleW
    offsetY = (root.height - orgHeight * scaleH) / 2
  }
  root.style.zoom = scaleW / 2
  update('resize')
}

function update() {
  generateHtmlBasedOnBlockNumber(blockNumber)
}

function generateSvgForDigits(blockNumber) {
    const originalString = blockNumber.toString()
    const digits = originalString.padStart(7, '0').split('').map(Number).reverse()
    let svgs = []


//background
if (originalString.length >= 1) {
const colorForFirstDigit = colorMap4[digits[0]]  // Example color variable, replace it with your logic
const background = colorForFirstDigit; // Set background color to colorForFourthDigit
    svgs.push(`
        <div style="position: absolute; top: 0px; left: 0px; z-index:0;"> 
		<svg width="425" height="425" viewBox="0 0 425 425" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="425" height="425" fill="${colorForFirstDigit}"/>
            </svg>
	      </div>
    `);
}



    //neck
    if (originalString.length >= 2) {
        const colorForSecondDigit = colorMap2[digits[1]] || "transparent"
        svgs.push(`
            <div style="position: absolute; top: 334px; left: 167px;">
                <svg width="92" height="91" viewBox="0 0 92 91" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <rect width="92" height="91" fill="${colorForSecondDigit}"/>
                </svg>
            </div>
        `)
    }
	
  


    //left top horn 
    if (originalString.length >= 1) {
        const colorForThirdDigit = colorMap2[digits[2]] || "transparent"
        svgs.push(`
            <div style="position: absolute; top: 59px; left: 100px;z-index:999">
                 <svg width="67" height="96" viewBox="0 0 67 96" fill="none" xmlns="http://www.w3.org/2000/svg">
               <!-- <path d="M6.99383e-06 0L80 80L0 80L6.99383e-06 0Z" fill="${colorForThirdDigit}"/>-->
				<path fill="${colorForThirdDigit}" d="M15.18,9.724C7.423,0.166-5.837-8.458,2.839,15.35
			c12.559,34.459,29.983,79.756,29.983,79.756l34-23C66.822,72.106,37.569,37.317,15.18,9.724z"/>
                </svg>
				
				
            </div>
        `)
    }
    //right top horn
    if (originalString.length >= 1) {
        const colorForFourthDigit = colorMap2[digits[2]] || "transparent"

        svgs.push(`
		<div style="position: absolute; top: 59px; left: 260px; z-index:999">
            <svg width="67" height="96" viewBox="0 0 67 96" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path d="M51.643,9.724c7.757-9.559,21.017-18.182,12.341,5.626
			C51.425,49.809,34,95.106,34,95.106l-34-23C0,72.106,29.253,37.317,51.643,9.724z" fill="${colorForFourthDigit}"/>
                </svg>
            </div>
        `)
    }
    //neck
    if (originalString.length >= 2) {
        const colorForSecondDigit = colorMap2[digits[1]] || "transparent"
        svgs.push(`
            <div style="position: absolute; top: 334px; left: 167px;">
                <svg width="92" height="91" viewBox="0 0 92 91" fill="none" xmlns="http://www.w3.org/2000/svg">
                    <rect width="92" height="91" fill="${colorForSecondDigit}"/>
                </svg>
            </div>
        `)
    }
	
	 // top-3fin
    if (originalString.length >= 1) {
        const colorForFirstDigit = colorMap[digits[3]]
		const colorForSecondDigit = colorMap[digits[2]];
        svgs.push(`<div style="position: absolute; top: 24px; left: 182px; z-index:999;">
                        <svg width="62" height="99" viewBox="0 0 62 99" fill="none" xmlns="http://www.w3.org/2000/svg">
<polygon fill-rule="evenodd" clip-rule="evenodd" points="29.363,0 0,89.076 61.722,89.076" fill="${colorForFirstDigit}" />
<polygon fill-rule="evenodd" clip-rule="evenodd" points="30.507,27.354 9.819,89.777 53.305,89.777" fill="${colorForSecondDigit}"/>
<polygon fill-rule="evenodd" clip-rule="evenodd" points="31.079,49.798 14.729,98.895 49.097,98.895" fill="${colorForFirstDigit}"/>
                        </svg>
	
            </div>
        `)
    }
	
	 // left-long-horn
    if (originalString.length >= 1) {
        const colorForFirstDigit = colorMap[digits[3]]
        svgs.push(`<div style="position: absolute; top: 5px; left: 26px;">
                        <svg width="98" height="170" viewBox="0 0 98 170" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd" clip-rule="evenodd" d="M34.424,1c-2.732,12.02-5.44,24.046-8.207,36.059
		c-1.208,5.248-2.74,10.432-3.683,15.722c-0.292,1.635,0.313,4.126,1.462,5.216c23.501,22.298,73.323,68.765,73.323,68.765
		l-41.273,43.204c0,0-55.206-110.057-56.047-111.306c0-0.86,0-1.721,0-2.581c8.513-13.698,17.063-27.372,25.514-41.107
		C28.33,10.396,30.888,5.662,33.563,1C33.851,1,34.138,1,34.424,1z" fill="${colorForFirstDigit}"/>
                        </svg>
	
            </div>
        `)
    }
	
 // right-long-horn
    if (originalString.length >= 1) {
        const colorForFirstDigit = colorMap[digits[3]]
        svgs.push(`<div style="position: absolute; top: 5px; left: 301px;">
                        <svg width="98" height="170" viewBox="0 0 98 170" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd" clip-rule="evenodd" d="M62.896,1c2.732,12.02,5.44,24.046,8.207,36.059
		c1.208,5.248,2.74,10.432,3.683,15.722c0.292,1.635-0.313,4.126-1.462,5.216C49.822,80.295,0,126.762,0,126.762l41.273,43.204
		c0,0,55.206-110.057,56.047-111.306c0-0.86,0-1.721,0-2.581c-8.513-13.698-17.063-27.372-25.514-41.107
		C68.991,10.396,66.433,5.662,63.757,1C63.47,1,63.183,1,62.896,1z" fill="${colorForFirstDigit}"/>
                        </svg>
	
            </div>
        `)
    }
	
	
	
 // forehead
    if (originalString.length >= 0) {
        const colorForFirstDigit = colorMap[digits[3]]
        svgs.push(` <div style="position: absolute; top: 175px; left:188px;z-index:97;">
                <svg width="50" height="55" viewBox="0 0 50 55" fill="none" xmlns="http://www.w3.org/2000/svg">
				 <polygon fill-rule="evenodd" clip-rule="evenodd" fill="#ffffff" points="24.506,12.287 0,0 0,10.423 24.506,22.71 49.014,10.423 49.014,0" fill-opacity="0.2"/>
				<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#ffffff" points="24.506,27.917 0,15.63 0,26.052 24.506,38.34 
			49.014,26.052 49.014,15.63" fill-opacity="0.2"/>
			<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#ffffff" points="24.506,43.868 0,31.581 0,42.003 24.506,54.291 
			49.014,42.003 49.014,31.581" fill-opacity="0.2"/>
			</svg>
	
            </div>
        `)
    }
	
	
 // eyes outliner 
    if (originalString.length >= 0) {
        const colorForFirstDigit = colorMap[digits[0]]
        svgs.push(` <div style="position: absolute; top: 182px; left: 104px;z-index:98;">
                 <svg width="216" height="92" viewBox="0 0 216 92" fill="none" xmlns="http://www.w3.org/2000/svg">
      
<path fill="${colorForFirstDigit}" d="M33.133,0C13.608,0,0,18.629,0,18.629v55.294c0,0,10.757,17.857,33.133,17.857
	c22.375,0,33.133-17.857,33.133-17.857V18.629C66.266,18.629,52.659,0,33.133,0z M56.656,68.79c0,0-7.637,12.678-23.523,12.678
	S9.61,68.79,9.61,68.79V23.536c0,0,9.661-13.225,23.523-13.225c13.861,0,23.523,13.225,23.523,13.225V68.79z" fill-opacity="0.2"/>
<path d="M56.656,68.791c0,0-7.637,12.678-23.523,12.678S9.61,68.791,9.61,68.791V23.537c0,0,9.661-13.225,23.523-13.225
	c13.861,0,23.523,13.225,23.523,13.225V68.791z"/>
<path d="M206.281,68.791c0,0-7.637,12.678-23.523,12.678c-15.885,0-23.523-12.678-23.523-12.678V23.537
	c0,0,9.662-13.225,23.523-13.225s23.523,13.225,23.523,13.225V68.791z"/>
<path fill="${colorForFirstDigit}" d="M182.758,0.031c-19.524,0-33.133,18.629-33.133,18.629v55.294c0,0,10.758,17.857,33.133,17.857
	s33.133-17.857,33.133-17.857V18.66C215.891,18.66,202.283,0.031,182.758,0.031z M206.281,68.821c0,0-7.637,12.678-23.523,12.678
	c-15.885,0-23.523-12.678-23.523-12.678V23.567c0,0,9.662-13.225,23.523-13.225s23.523,13.225,23.523,13.225V68.821z" fill-opacity="0.2"/> 
			
			
			</svg>
			
			
	
            </div>
			
			
			
			
        `)
    }			
		
			
			
    // face
    if (originalString.length >= 1) {
        const colorForFirstDigit = colorMap[digits[0]]
        svgs.push(`<div style="position: absolute; top: 112px; left: 81px;">
                        <svg width="263" height="233" viewBox="0 0 263 233" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <path fill-rule="evenodd" clip-rule="evenodd" d="M203.533,5.165L197.078,0c0,0-120.054,0-130.812,0C55.509,0,7.167,54.442,0,61.965
		c0,24.116,0,75.303,0,108.437c15.061,32.271,76.163,50.774,99.399,54.217c9.468,5.594,16.783,7.747,32.274,7.747
		c26.393,0,30.98-7.747,30.98-7.747c18.646,2.296,97.68-38.296,99.83-53.357c0-34.423,0-86.49,0-109.153
		C249.146,42.458,203.533,5.165,203.533,5.165z M89.631,143.928c0,0-10.758,17.857-33.133,17.857
		c-22.376,0-33.133-17.857-33.133-17.857V88.634c0,0,13.608-18.629,33.133-18.629c19.525,0,33.133,18.629,33.133,18.629V143.928z
		 M239.119,143.928c0,0-10.758,17.857-33.133,17.857s-33.133-17.857-33.133-17.857V88.634c0,0,13.609-18.629,33.133-18.629
		c19.525,0,33.133,18.629,33.133,18.629V143.928z" fill="${colorForFirstDigit}"/>
		

			
                        </svg>
						
	
            </div>
        `)
    }
	
	 // Wings
    if (originalString.length >= 1) {
        const colorForFirstDigit = colorMap[digits[0]]
        svgs.push(`<div style="position: absolute; top: 343px; left: 2px;">
                        <svg width="423" height="83" viewBox="0 0 423 83" fill="none" xmlns="http://www.w3.org/2000/svg">
           <polygon fill="${colorForFirstDigit}" points="44.491,59.812 81.014,58.475 89.271,79.646 151.481,1.694" fill-opacity="0.7"/>
	<polygon fill="${colorForFirstDigit}" points="143.389,0 0,0.076 44.59,24.481 41.513,54.598" />
	<polygon fill="${colorForFirstDigit}" points="159.25,3.487 94.61,82.152 159.25,82.152" />
		<polygon fill="${colorForFirstDigit}" points="377.759,59.812 341.236,58.475 332.979,79.646 270.769,1.694" fill-opacity="0.7"/>
		<polygon fill="${colorForFirstDigit}" points="278.861,0 422.25,0.076 377.66,24.481 380.737,54.598" />
				<polygon fill="${colorForFirstDigit}" points="263,3.487 327.64,82.152 263,82.152 " />
					   </svg>
	
            </div>
        `)
    }
	
	//side horns
    if (originalString.length >= 1) {
        const colorForFirstDigit = colorMap[digits[0]]
        svgs.push(`<div style="position: absolute; top: 175px; left: 37px;">
                        <svg width="351" height="106" viewBox="0 0 351 106" fill="none" xmlns="http://www.w3.org/2000/svg">
                      <polygon fill-rule="evenodd" clip-rule="evenodd"  fill="${colorForFirstDigit}" points="10.451,18.029 42.138,32.989 0,54.115 40.528,73.006 9.451,89.172 44.106,105.533 44.106,0" fill-opacity="0.7" />
				<polygon fill-rule="evenodd" clip-rule="evenodd"  fill="${colorForFirstDigit}" points="339.657,18.029 307.97,32.989 350.107,54.115 309.579,73.006 340.657,89.172 306.001,105.533 306.001,0" fill-opacity="0.7"/>
                        </svg>
						
					
	
            </div>
        `)
    }
    //nose//use5
    if (originalString.length >= 1) {
        const colorForFifthDigit = colorMap2[digits[4]] || "transparent"
        svgs.push(`
            <div style="position: absolute; top: 248px; left: 192px;">
                <svg width="44" height="18" viewBox="0 0 44 18" fill="none" xmlns="http://www.w3.org/2000/svg">
               
				<path d="M16.158,7.878c2.286,2.286,2.286,5.993,0,8.279l0,0c-2.286,2.286-5.993,2.286-8.279,0L1.715,9.994
	c-2.286-2.286-2.286-5.993,0-8.279l0,0c2.286-2.286,5.993-2.286,8.279,0L16.158,7.878z" fill="#333333"/>
<path d="M27.715,7.878c-2.286,2.286-2.286,5.993,0,8.279l0,0c2.286,2.286,5.993,2.286,8.279,0l6.164-6.164
	c2.286-2.286,2.286-5.993,0-8.279l0,0c-2.286-2.286-5.993-2.286-8.279,0L27.715,7.878z" fill="#333333"/>
				
                </svg>
            </div>
        `)
    }

    //stripes
    if (originalString.length >= 0) {
        const colorForSixthDigit = colorMap[digits[1]] || "transparent"
        svgs.push(`
            <div style="position: absolute; top: 345px; left:172px;"> 
                <svg width="83" height="80" viewBox="0 0 83 80" fill="none" xmlns="http://www.w3.org/2000/svg">
              <g>
	<polygon points="44.11,21.912 0,0 0,18.586 44.11,40.5 88.225,18.586 88.225,0 " fill="${colorForSixthDigit}"/>
	<g><polygon points="44.11,55.412 0,33.5 0,52.086 44.11,74 88.225,52.086 88.225,33.5" fill="${colorForSixthDigit}"/></g>
	<!--<g><polygon points="44.29,89.412 0.18,67.5 0.18,86.086 44.29,108 88.404,86.086 88.404,67.5" fill="${colorForSixthDigit}"/></g>-->
</g>

				
				
				
                </svg>
            </div>

        `)
    }



    //tail
   if (originalString.length >= 1) {
        const colorForFirstDigit = colorMap[digits[0]] 
        svgs.push(`
            <div style="position: absolute; top: 280px; left:340px;"> 
                <svg width="78" height="65" viewBox="0 0 78 65" fill="none" xmlns="http://www.w3.org/2000/svg">
                <polygon fill-rule="evenodd" clip-rule="evenodd" fill="${colorForFirstDigit}" points="77.332,16.272 58.606,19.287 54.94,0.724 45.438,13.825 49.154,29.858 38.554,37.374 31.243,33.043 30.448,43.122 19.428,50.936 8.419,44.416 7.224,59.59 0,64.712 40.565,64.712 53.662,32.842 69.64,30.448" fill-opacity="0.7"/>
                </svg>
            </div>

        `)
    }


    return svgs.join('')
}

function generateSvgForDigits2(blockNumber) {
    const originalString = blockNumber.toString()
    const digits = originalString.padStart(7, '0').split('').map(Number).reverse()
    let svgs = []


    //toothL/used check brithmark/
    if (originalString.length >= 7) {
        const colorForSeventhDigit = colorMap3[digits[6]] || "FFFFFF"
        svgs.push(`
            <div style="position: absolute; top: 280px; left:115px; z-index:9999;">
                <svg width="35" height="21" viewBox="0 0 35 21" fill="none" xmlns="http://www.w3.org/2000/svg">
<circle fill-rule="evenodd" clip-rule="evenodd" fill="#CCCCCC" cx="5.335" cy="5.335" r="5.335" fill-opacity="0.1"/>
<circle fill-rule="evenodd" clip-rule="evenodd" fill="#CCCCCC" cx="24.276" cy="5.335" r="5.335" fill-opacity="0.1"/>
<circle fill-rule="evenodd" clip-rule="evenodd" fill="#CCCCCC" cx="14.832" cy="14.877" r="5.335" fill-opacity="0.1"/> 

                </svg>
            </div>
        `)
    }

    //toothR/used check brithmark/
    if (originalString.length >= 7) {
        const colorForSeventhDigit = colorMap3[digits[6]] || "FFFFFF"
        svgs.push(`
            <div style="position: absolute; top: 280px; left:280px; z-index:9999;">
                 <svg width="40" height="35" viewBox="0 0 40 35" fill="none" xmlns="http://www.w3.org/2000/svg">
				 
<circle fill="${colorForSeventhDigit}"cx="5.335" cy="5.335" r="5.335" fill-opacity="0.1"/>
<circle fill="${colorForSeventhDigit}" cx="24.276" cy="5.335" r="5.335" fill-opacity="0.1"/>
<circle fill="${colorForSeventhDigit}" cx="14.832" cy="14.877" r="5.335" fill-opacity="0.1"/> 

                </svg>
            </div>

        `)
    }


    return svgs.join('')
}

//eye direction
function getLook(number) {
    const numberStr = number.toString()
    const lastFourDigits = parseInt(numberStr.substring(numberStr.length - 4))

    if (lastFourDigits < 4800) {
        return "look_right"
    } else if (lastFourDigits >= 4800 && lastFourDigits <= 5200) {
        return "look_crossed"
    } else {
        return "look_left"
    }
}

function generateLookSvg(lookDir) {
    let lookHtml = ""
    if (lookDir === "look_left") {
        lookHtml = `<div style="position: absolute; top: 195px; left:123px;z-index:99">
                        <svg width="158" height="38" viewBox="0 0 158 38" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect x="44" y="38" width="10" height="16" transform="rotate(-180 37 38)" fill="#FFFFFF" fill-opacity="0.5"/>
                        <rect x="157" y="38" width="10" height="16" transform="rotate(-180 158 38)" fill="#FFFFFF" fill-opacity="0.5"/>
                        </svg>
                    </div>`
    } else if (lookDir === "look_right") {
        lookHtml = `<div style="position: absolute; top: 195px; left: 123px;z-index:99">
                        <svg width="158" height="38" viewBox="0 0 158 38" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect x="65" y="45" width="15" height="16" transform="rotate(-180 37 38)" fill="#FFFFFF" fill-opacity="0.5"/>
                        <rect x="151" y="45" width="15" height="16" transform="rotate(-180 157.5 38)" fill="#FFFFFF" fill-opacity="0.5"/>
                        </svg>
                    </div>`
    } else if (lookDir === "look_crossed") {
        lookHtml = `<div style="position: absolute; top: 195px; left: 133px;z-index:99">
                        <svg width="195" height="38" viewBox="0 0 195 38" fill="none" xmlns="http://www.w3.org/2000/svg">
                        <rect x="240" y="38" width="15" height="16" transform="rotate(-180 194.5 38)" fill="#FFFFFF" fill-opacity="0.5"/>
                        <rect x="50" y="38" width="15" height="16" transform="rotate(-180 37 38)" fill="#FFFFFF" fill-opacity="0.5"/>
                        </svg>
                    </div>`
    }
    return lookHtml
}

function c420(number) {
    const numberStr = number.toString()
    return numberStr.includes('420')
}

//used for top horn topHelo
function displaytopHelo() {
    return `<div style="position: absolute; top: 41px; left: 171px;z-index:9999">
        <svg width="81" height="17" viewBox="0 0 81 17" fill="none" xmlns="http://www.w3.org/2000/svg">
       <path fill="#FF9900" d="M49.791,0.05l-2.518,2.425c12.292,0.64,21.393,2.938,21.393,5.676c0,3.233-12.686,5.854-28.333,5.854
		S12,11.384,12,8.151c0-2.803,9.541-5.146,22.279-5.719L31.937,0C13.691,0.798,0,4.144,0,8.151c0,4.603,18.058,8.333,40.333,8.333
		s40.333-3.731,40.333-8.333C80.666,4.222,67.501,0.93,49.791,0.05z"/>
        </svg>
    </div>`
}

function c4a0(number) {
    const numberStr = number.toString()
    return numberStr.includes('4') && numberStr.includes('0')
}

function displayfire(number) {
    return `<div style="position: absolute; top: 295px; left: 182px; z-index:999;">
        <svg width="61" height="105" viewBox="0 0 61 105" fill="none" xmlns="http://www.w3.org/2000/svg">
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FFFF00" points="21.439,92.789 10.521,14.289 32.573,14.289 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FF0000" points="16.845,83.013 26.663,27.643 45.698,34.703 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FF0000" points="48.698,83.013 38.88,27.643 19.845,34.703 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FFFF00" points="39.558,92.789 29.271,14.289 50.048,14.289 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#F88D13" points="31.117,107.079 13.771,0 46.771,0 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FF0000" points="15.051,55.328 5,0 25.302,0 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FF0000" points="46.293,55.328 36.242,0 56.543,0 "/>
        </svg>
    </div>`
}

function c0(number) {
    const numberStr = number.toString()
    return numberStr.includes('0')
}


function displayEarring(number) {
    return `<div style="position: absolute; top: 120px; left: 87px;">
                <svg width="19" height="18" viewBox="0 0 19 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="19" height="18" fill="#FFC700"/>
                </svg>
    </div>`
}

function c00(number) {
    const numberStr = number.toString()
    return numberStr.includes('00')
}

function displayEarringR(number) {
    return `<div style="position: absolute; top: 120px; left: 319px;">
                <svg width="19" height="18" viewBox="0 0 19 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="19" height="18" fill="#FFC700"/>
                </svg>
    </div>`
}

function c000(number) {
    const numberStr = number.toString()
    return numberStr.includes('000')
}

function displayAlienEarring(number) {
    return `<div style="position: absolute; top: 120px; left: 87px;">
                <svg width="19" height="18" viewBox="0 0 19 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="19" height="18" fill="#49EFEF"/>
                </svg>
    </div>`
}

function c0000(number) {
    const numberStr = number.toString()
    return numberStr.includes('0000')
}

function displayAlienEarringR(number) {
    return `<div style="position: absolute; top: 120px; left: 319px;">
                <svg width="19" height="18" viewBox="0 0 19 18" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="19" height="18" fill="#49EFEF"/>
                </svg>
    </div>`
}

function c00000(number) {
    const numberStr = number.toString()
    return numberStr.includes('00000')
}

function displayAlienTiara(number) {
    return `<div style="position: absolute; top: 102px; left: 132px;">
                <svg width="162" height="36" viewBox="0 0 162 36" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M36 0H54V18H36V0ZM36 18V36H18H0V18H18H36ZM72 18V36H54V18H72ZM90 18H72V0H90V18ZM108 18V36H90V18H108ZM126 18H108V0H126V18ZM144 18H126V36H144H162V18H144Z" fill="#00FFF0"/>
                </svg>  
    </div>`
}

function c11(number) {
    const numberStr = number.toString()
    return numberStr.includes('11')
}

//used for butterfly
function displayButterFly(number) {
    return `<div style="position: absolute; top: 100px; left: 325px;">
                <svg width="47" height="44" viewBox="0 0 47 44" fill="none" xmlns="http://www.w3.org/2000/svg">
                <polygon fill="#4B0082" points="18.325,3.5 15.98,11.22 7.935,10.609 22.677,21.009 "/>
<polygon fill="#FF0000" points="30.918,0.678 18.008,14.333 0.618,21.457 23.271,22.006 "/>
<polygon fill="#FF0000" points="45.733,22.835 28.344,29.959 15.433,43.614 23.082,22.286 "/>
                </svg>
    </div>`
}

function c111(number) {
    const numberStr = number.toString()
    return numberStr.includes('111')
}
//horn ring
function displayHornRing(number) {
    return `<div style="position: absolute; top: 80px; left: 281px;z-index:9999">
        <svg width="38" height="39" viewBox="0 0 38 39" fill="none" xmlns="http://www.w3.org/2000/svg">
      <!--  <rect x="18" y="18" width="18" height="18" transform="rotate(-180 18 18)" fill="#FFC700"/>-->
<path fill-rule="evenodd" clip-rule="evenodd" fill="#FFCD05" d="M0,22.614c0,0,4.966,4.413,12.622,8.896s15.418,7.238,15.418,7.238
	l9.617-28.817c0,0-3.943-0.051-10.155-3.703S20.183,0,20.183,0L0,22.614z"/>
<path fill-rule="evenodd" clip-rule="evenodd" fill="#F8991D" d="M15.189,19.771c1.808-2.588,3.614-5.179,5.42-7.77
	c0.063-0.073,0.057-0.135-0.031-0.184c-0.035-0.034-0.07-0.071-0.106-0.104c-0.429-0.403-1.41-0.651-1.287-1.093
	c0.209-0.757,0.738-1.505,1.447-2.017c0.709,0.565,1.445,1.09,2.229,1.548c0.048,0.099,0.099,0.191,0.229,0.101
	c0.035-0.034,0.068-0.069,0.103-0.104c0.165-0.219,0.37-0.419,0.488-0.66c0.301-0.612,0.622-0.609,1.147-0.213
	c0.429,0.325,0.588,0.56,0.186,0.992c-0.168,0.183-0.356,0.389-0.421,0.616c-0.263,0.926,0.718,0.862,1.092,1.285
	c0.191,0.216,0.517,0.584,0.83,0.054c0.114-0.193,0.282-0.357,0.38-0.557c0.288-0.593,0.601-0.526,1.095-0.186
	c0.496,0.343,0.567,0.621,0.165,1.05c-0.101,0.108-0.163,0.253-0.262,0.365c-0.414,0.471-0.512,0.876-0.144,1.508
	c0.916,1.565,0.668,4.159-2.175,5.362c1.28,2.612,0.504,5.166-1.944,6.506c-1.301,0.713-2.602,0.47-3.916,0.111
	c-0.078-0.021-0.136-0.132-0.192-0.209c-0.353-0.479-0.662-0.45-0.94,0.07c-0.047,0.088-0.113,0.166-0.17,0.248
	c-0.499,0.71-0.965,0.705-1.544,0.019c-0.253-0.301,0.013-0.457,0.133-0.642c0.679-1.049,0.684-1.046-0.327-1.751
	c-0.881-0.615-0.914-0.637-1.53,0.315c-0.338,0.523-0.604,0.492-1.056,0.162c-0.424-0.31-0.638-0.519-0.238-1.044
	c0.697-0.914,0.661-0.942-0.254-1.581c-0.021-0.014-0.042-0.028-0.062-0.042c-1.754-1.223-1.747-1.218-0.374-2.876
	c0.21-0.253,0.357-0.275,0.609-0.092c0.403,0.294,0.827,0.561,1.241,0.841C15.085,19.866,15.144,19.857,15.189,19.771z
	 M24.351,14.818c-0.415-0.372-0.866-0.693-1.357-0.956c-0.839,0.614-1.224,1.576-1.813,2.38c0.31,0.609,0.942,0.836,1.465,1.167
	c0.906,0.573,1.764,0.438,2.266-0.263c0.498-0.693,0.332-1.505-0.457-2.238C24.435,14.861,24.4,14.831,24.351,14.818z
	 M20.088,18.812c-0.291-0.294-0.525-0.39-0.799,0.063c-0.374,0.618-0.839,1.181-1.224,1.792c-0.184,0.292-0.566,0.497-0.472,0.933
	c0.73,0.52,1.447,1.058,2.193,1.552c0.844,0.559,2.024,0.591,2.685-0.357c0.643-0.922,0.47-1.936-0.541-2.717
	C21.343,19.623,20.704,19.231,20.088,18.812z"/>
<path fill-rule="evenodd" clip-rule="evenodd" fill="#F8991D" d="M38.944,32.978"/>
<path fill-rule="evenodd" clip-rule="evenodd" fill="#F8991D" d="M72.333,48.123c0.052,0.082,0.03,0.138-0.061,0.167
	C72.292,48.234,72.313,48.178,72.333,48.123z"/>
<path fill-rule="evenodd" clip-rule="evenodd" fill="#F8991D" d="M44.587,37.743"/>
<path fill-rule="evenodd" clip-rule="evenodd" fill="#F8991D" d="M44.622,36.773"/>
<path fill-rule="evenodd" clip-rule="evenodd" fill="#F8991D" d="M75.106,46.924"/>
<path fill-rule="evenodd" clip-rule="evenodd" fill="#F8991D" d="M43.362,31.052l-0.057,0.044l-0.073,0.002
	C43.268,31.063,43.311,31.047,43.362,31.052z"/>
		
		
		
        </svg>
    </div>`
}


function c1111(number) {
    const numberStr = number.toString()
    return numberStr.includes('1111')
}

function displayFlysAlienEarring(number) {
    return `<div style="position: absolute; top: 178px; left: 378px;">
        <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
        <rect x="18" y="18" width="18" height="18" transform="rotate(-180 18 18)" fill="#49EFEF"/>
        </svg>
    </div>`
}


function c11111(number) {
    const numberStr = number.toString()
    return numberStr.includes('11111')
}


function displayFlysLaserEyes(number) {
    return `<div style="position: absolute; top: 178px; left: 0;">
                <svg width="340" height="19" viewBox="0 0 340 19" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="340" height="18.5" fill="#FD1935"/>
                </svg>
            </div>`
}

function c8a8(number) {
    const numberStr = number.toString()
    const count = (numberStr.match(/8/g) || []).length
    return count >= 2 ? "c8a8" : null
}


function displayBowR(number) {
    return `<div style="position: absolute; top:65px; left: 95px; z-index:9999;"> 
        <svg width="52" height="52" viewBox="0 0 52 52" fill="none" xmlns="http://www.w3.org/2000/svg">
	<polygon fill="#0000FF" points="39.018,51.754 26.129,44.059 13.241,51.754 26.129,25.976 	"/>
	<polygon fill="#FF0000" points="38.991,0 26.102,7.695 13.213,0 26.102,25.777 	"/>
	<polygon fill="#007F70" points="10.849,1.813 12.406,11.978 2.244,10.398 26.175,25.777 	"/>
	<polygon fill="#4B0082" points="10.556,49.94 12.114,39.775 1.952,41.355 25.883,25.977 	"/>
	<polygon fill="#FFA500" points="41.502,1.912 39.945,12.077 50.106,10.497 26.175,25.877 	"/>
	<polygon fill="#008000" points="41.794,50.039 40.237,39.875 50.398,41.455 26.467,26.075 	"/>
	<polygon fill="#EE82EE" points="0.352,12.889 8.046,25.777 0.352,38.666 26.129,25.777 	"/>
	<polygon fill="#FFFF00" points="51.879,38.666 44.185,25.777 51.879,12.889 26.102,25.777 	"/>
        </svg>
    </div>`
}

function c88(number) {
    const numberStr = number.toString()
    return numberStr.includes('88')
}

//used for flower
function displayFlowerBadge(number) {
    return `<div style="position: absolute; top: 65px; left: 280px;z-index:9999;">
        <svg width="52" height="52" viewBox="0 0 52 52" fill="none" xmlns="http://www.w3.org/2000/svg">
	<polygon fill="#0000FF" points="39.018,51.754 26.129,44.059 13.241,51.754 26.129,25.976 	"/>
	<polygon fill="#FF0000" points="38.991,0 26.102,7.695 13.213,0 26.102,25.777 	"/>
	<polygon fill="#007F70" points="10.849,1.813 12.406,11.978 2.244,10.398 26.175,25.777 	"/>
	<polygon fill="#4B0082" points="10.556,49.94 12.114,39.775 1.952,41.355 25.883,25.977 	"/>
	<polygon fill="#FFA500" points="41.502,1.912 39.945,12.077 50.106,10.497 26.175,25.877 	"/>
	<polygon fill="#008000" points="41.794,50.039 40.237,39.875 50.398,41.455 26.467,26.075 	"/>
	<polygon fill="#EE82EE" points="0.352,12.889 8.046,25.777 0.352,38.666 26.129,25.777 	"/>
	<polygon fill="#FFFF00" points="51.879,38.666 44.185,25.777 51.879,12.889 26.102,25.777 	"/>
        </svg>
    </div>`
}

function c888(number) {
    const numberStr = number.toString()
    return numberStr.includes('888')
}

//displayBowTail used for crown
function displayHeadCrown(number) {
    return `<div style="position: absolute; top:50px; left: 81px; z-index:9999;">
        <svg width="263" height="126" viewBox="0 0 263 126" fill="none" xmlns="http://www.w3.org/2000/svg">
        
		<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FF9933" points="211.553,59.464 190.605,39.208 166.857,61.879 
	136.909,12.917 106.183,62.249 83.211,40.034 59.742,62.438 0,0 0,124.981 262.107,125.104 262.107,0 "/>
<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="136.85" cy="89.677" r="25.125"/>
	<path fill="#FF9933" d="M127.687,80.764c-1.263,0.741-2.584,1.359-3.938,1.91c0.158,1.401,0.791,2.736,1.689,3.63
		c0.522,0.522,1.647-0.66,2.555-0.944c0.078-0.025,0.153-0.045,0.231-0.068c0.137-0.084,0.23-0.045,0.284,0.103
		c2.21,4.59,4.422,9.179,6.633,13.768c0.083,0.132,0.049,0.222-0.101,0.27c-0.719,0.363-1.43,0.746-2.162,1.08
		c-0.459,0.211-0.544,0.433-0.353,0.929c1.244,3.243,1.23,3.249,4.336,1.751c0.038-0.018,0.073-0.035,0.11-0.053
		c1.622-0.781,1.689-0.81,2.424,0.895c0.422,0.978,0.883,0.842,1.654,0.491c0.823-0.374,1.078-0.724,0.61-1.613
		c-0.851-1.618-0.796-1.647,0.768-2.4c1.789-0.862,1.782-0.859,2.718,0.926c0.164,0.314,0.174,0.812,0.797,0.694
		c1.424-0.271,1.8-0.923,1.194-2.186c-0.069-0.146-0.126-0.301-0.21-0.436c-0.51-0.802-0.307-1.261,0.646-1.377
		c0.151-0.018,0.354-0.012,0.444-0.104c1.546-1.564,2.916-3.2,2.943-5.593c0.054-4.501-2.923-7.613-7.608-7.881
		c0.56-4.949-2.894-7.349-5.817-7.301c-1.179,0.019-1.674-0.439-2.006-1.395c-0.081-0.227-0.234-0.428-0.308-0.657
		c-0.282-0.904-0.729-1.025-1.603-0.599c-0.871,0.424-1.211,0.813-0.607,1.687c0.204,0.295,0.302,0.661,0.482,0.974
		c0.497,0.86-0.278,1.026-0.732,1.125c-0.89,0.19-1.576,1.621-2.671,0.518c-0.269-0.271-0.41-0.698-0.531-1.079
		c-0.291-0.907-0.749-0.871-1.542-0.524c-0.974,0.425-1.231,0.874-0.61,1.781c0.245,0.359,0.365,0.805,0.542,1.209
		c0.02,0.076,0.043,0.151,0.064,0.227C128.035,80.777,127.865,80.775,127.687,80.764z M145.151,91.317
		c0.81,1.68-0.17,3.313-1.622,4.058c-1.286,0.658-2.609,1.238-3.916,1.854c-0.688-0.211-0.675-0.912-0.94-1.4
		c-0.554-1.025-0.978-2.125-1.549-3.138c-0.42-0.744-0.101-0.999,0.542-1.174c1.079-0.533,2.134-1.122,3.241-1.589
		C142.801,89.126,144.364,89.684,145.151,91.317z M135.426,82.364c0.057-0.059,0.127-0.084,0.207-0.076
		c1.656-0.527,2.928-0.118,3.509,1.13c0.588,1.26,0.101,2.574-1.423,3.394c-0.879,0.473-1.698,1.182-2.801,1.136
		c-0.665-1.466-1.71-2.766-1.909-4.432C133.767,83.033,134.574,82.652,135.426,82.364z"/>
	<path fill="#FF9933" d="M141.28,93.206c-0.081-0.009-0.152,0.017-0.208,0.076l0.115-0.003L141.28,93.206z"/>

		
		
		
		
		
        </svg>
    </div>`
}

function c101(number) {
    const numberStr = number.toString()
    return numberStr.includes('101')
}

//chest flower 
function displayChestFlower (number) {
    return `<div style="position: absolute; top: 350px; left: 70px;">
    <svg width="52" height="52" viewBox="0 0 52 52" fill="none" xmlns="http://www.w3.org/2000/svg">
	<polygon fill="#0000FF" points="39.018,51.754 26.129,44.059 13.241,51.754 26.129,25.976 	"/>
	<polygon fill="#FF0000" points="38.991,0 26.102,7.695 13.213,0 26.102,25.777 	"/>
	<polygon fill="#007F70" points="10.849,1.813 12.406,11.978 2.244,10.398 26.175,25.777 	"/>
	<polygon fill="#4B0082" points="10.556,49.94 12.114,39.775 1.952,41.355 25.883,25.977 	"/>
	<polygon fill="#FFA500" points="41.502,1.912 39.945,12.077 50.106,10.497 26.175,25.877 	"/>
	<polygon fill="#008000" points="41.794,50.039 40.237,39.875 50.398,41.455 26.467,26.075 	"/>
	<polygon fill="#EE82EE" points="0.352,12.889 8.046,25.777 0.352,38.666 26.129,25.777 	"/>
	<polygon fill="#FFFF00" points="51.879,38.666 44.185,25.777 51.879,12.889 26.102,25.777 	"/>
        </svg>
    </div>`
}

function c696(number) {
    const numberStr = number.toString()
    return numberStr.includes('696')
}

function displayDoubleChestFlower (number) {
    return `<div style="position: absolute; top: 350px; left: 303px;">
        <svg width="52" height="52" viewBox="0 0 52 52" fill="none" xmlns="http://www.w3.org/2000/svg">
	<polygon fill="#0000FF" points="39.018,51.754 26.129,44.059 13.241,51.754 26.129,25.976 	"/>
	<polygon fill="#FF0000" points="38.991,0 26.102,7.695 13.213,0 26.102,25.777 	"/>
	<polygon fill="#007F70" points="10.849,1.813 12.406,11.978 2.244,10.398 26.175,25.777 	"/>
	<polygon fill="#4B0082" points="10.556,49.94 12.114,39.775 1.952,41.355 25.883,25.977 	"/>
	<polygon fill="#FFA500" points="41.502,1.912 39.945,12.077 50.106,10.497 26.175,25.877 	"/>
	<polygon fill="#008000" points="41.794,50.039 40.237,39.875 50.398,41.455 26.467,26.075 	"/>
	<polygon fill="#EE82EE" points="0.352,12.889 8.046,25.777 0.352,38.666 26.129,25.777 	"/>
	<polygon fill="#FFFF00" points="51.879,38.666 44.185,25.777 51.879,12.889 26.102,25.777 	"/>
        </svg>
    </div>`
}


function cp6(number) {
    const numberStr = number.toString()
    // Loop through the number string
    for (let i = 0; i <= numberStr.length - 6; i++) { // Ensure there are at least 6 characters to check
        const substring = numberStr.substring(i, i + 6) // Get the substring of 6 characters
        // Check if the substring is a palindrome
        if (substring === substring.split('').reverse().join('')) {
            return "cp6"
        }
    }
    return null
}
//used for diamond
function displayAlienDiamond(number) {
    return `<div style="position: absolute; top:300px; left: 325px;">
        <svg width="59" height="44" viewBox="0 0 59 44" fill="none" xmlns="http://www.w3.org/2000/svg">
       <polygon fill="#49EFEF" points="43.351,0 14.564,0 0,14.027 28.409,43.526 57.909,15.118 	"/>
        </svg>
    </div>`
}


function c9a9(number) {
    const numberStr = number.toString()
    const count = (numberStr.match(/9/g) || []).length
    return count >= 2 ? "c9a9" : null
}

function displayFish(number) {
    return `<div style="position: absolute; top: 290px; left: 150px; z-index:9999;">
                <svg width="126px" height="62px" viewBox="0 0 126 62" fill="none" xmlns="http://www.w3.org/2000/svg">
                
	<path fill-rule="evenodd" clip-rule="evenodd" fill="#3CDAFD" d="M123.984,14.55c0,4.333-9.484,17.117-12,20.999
		C73.527,35.552,45.07,35.569,6.613,35.513c-1.889-0.003-3.777-0.652-5.666-1C9.244,23.858,20.508,18.32,33.426,15.561
		c2.729-0.583,5.458-1.321,8.063-2.313c7.273-2.771,14.486-5.701,21.714-8.594c1.618-0.647,3.194-1.398,4.789-2.102
		C69.324,2.553,70.667,1,72,1c1.031,3.639,2.271,8.788,3.015,12.485c0.586,2.92,1.822,4.437,4.81,5.431
		c6.122,2.038,12.114,4.538,17.982,7.236c2.687,1.234,4.604,1.15,7.304-0.179C110.339,23.399,116.75,17.053,123.984,14.55z"/>
	<path fill-rule="evenodd" clip-rule="evenodd" fill="#ABEAF8" d="M0.947,34.513c1.889,0.348,3.777,0.997,5.666,1
		c38.457,0.057,66.914,0.039,105.371,0.036c0,4.999,11,13.998,11,18.998c-5.858-1.581-12.88-8.771-18.484-10.994
		c-2.866-1.137-4.985,0.704-7.872,1.61c-8.529,2.68-17.141,5.153-25.835,7.223c-2.835,0.675-1.188,1.538-0.891,4.431
		c0.49,4.787-1.85,5.476-5.7,3.916c-3.687-1.491-10.336-3.076-14.046-4.507c-1.824-0.704-3.718-1.696-5.63-1.654
		C15,55.22,0,35.216,0,34.549C0.315,34.524,0.631,34.512,0.947,34.513z"/>

</svg>					
    </div>`
}

function c869(number) {
    const numberStr = number.toString()
    return numberStr.includes('869')
}
//
function displayCat(number) {
    return `<div style="position: absolute; top: 285px; left: 25px; z-index:999;">
	<svg width="55" height="65" viewBox="0 0 55 65" fill="none" xmlns="http://www.w3.org/2000/svg">
	<path fill-rule="evenodd" clip-rule="evenodd" fill="#FF9933" d="M21.859,58.643c-1.409-2.979-3.177-5.898-4.1-9.064
	c-0.382-1.309,0.618-3.952,1.753-4.56c1.871-1.002,1.896-2.291,2.215-3.878c0.82-4.104,1.84-8.168,2.691-12.266
	c0.213-1.018,0.527-2.666,0.039-3.041c-0.991-0.76-2.61-1.361-3.768-1.105c-2.338,0.514-4.539,1.642-7.184,1.839
	c1.989-1.049,3.979-2.098,5.969-3.146c-0.035-0.17-0.07-0.339-0.105-0.509c-2.285,0.407-4.57,0.813-6.855,1.22
	c-0.066-0.249-0.133-0.498-0.2-0.746c1.981-0.622,3.964-1.244,6.731-2.112c-2.771-0.46-4.812-0.797-6.852-1.135
	c0.027-0.253,0.054-0.508,0.08-0.762c1.645,0,3.287,0,5.491,0c-0.63-5.474-1.218-10.512-1.788-15.551
	c-0.383-3.383,1.002-4.843,3.536-3.063c6.119,4.296,13.033,2.5,19.567,3.452c0.943,0.139,2.041-0.325,2.98-0.718
	c1.399-0.585,2.664-1.534,4.092-1.997c1.584-0.513,3.288-0.651,4.939-0.953c0.174,1.578,0.599,3.177,0.473,4.73
	c-0.354,4.372-0.935,8.726-1.453,13.333c1.951,0.37,3.43,0.649,4.909,0.93c0.003,0.21,0.005,0.42,0.009,0.63
	c-1.963,0.311-3.926,0.621-6.949,1.098c2.813,0.851,4.602,1.393,6.391,1.934c-0.06,0.272-0.119,0.544-0.178,0.816
	c-2.164-0.367-4.327-0.735-6.49-1.103c-0.06,0.167-0.119,0.335-0.18,0.503c1.945,1.004,3.89,2.008,5.834,3.012
	c-3.828,0.23-7.248-4.091-10.699-0.451c1.727,6.344,3.389,12.448,5.102,18.74c4.438-0.115,6.22,2.886,3.648,7.613
	c-3.938,7.243-9.689,11.583-18.305,11.318c-2.805-0.088-5.624,0.117-8.422-0.054C14.47,62.963,9.835,56.536,11.872,46.4
	c0.721-3.582,0.953-7.389,0.615-11.02c-0.279-3.005-3.119-4.289-5.459-2.924c-0.971,0.565-1.895,1.86-2.047,2.936
	c-0.09,0.633,1.385,1.478,2.141,2.241c0.855,0.865,1.693,1.746,2.539,2.62c-1.245,0.389-2.523,1.182-3.729,1.085
	c-2.824-0.229-5.021-3.271-4.93-6.313c0.11-3.737,3.469-7.366,7.021-7.584c4.267-0.263,8.284,2.843,9.026,7.69
	c0.449,2.924-0.029,6.01-0.252,9.011c-0.182,2.436-0.914,4.862-0.85,7.278C16.058,55.521,18.017,57.582,21.859,58.643z"/>
</svg>

    </div>`
}
function c999(number) {
    const numberStr = number.toString()
    return numberStr.includes('999')
}
//used for BtcKey
function displayBtcKey(number) {
    return `<div style="position: absolute; top: 280px; left: 130px;">
                <svg width="200" height="70" viewBox="0 0 212 76" fill="none" xmlns="http://www.w3.org/2000/svg">
               <path fill="#FF9900" d="M165.348,0c-11.254,0-21.238,5.428-27.482,13.808h-8.531v4.674h-7.545v7.691h-62.25v-7.212H48.674v7.212H0
	v18.269h6.366v25H18.28V48.692h11.691v20.749h13.895v-25h4.808v6.25h10.865v-6.25h62.25v5.771h7.545v3.979h8.175
	c6.215,8.661,16.363,14.309,27.838,14.309c18.915,0,34.25-15.334,34.25-34.25S184.263,0,165.348,0z M165.348,53.667
	c-10.725,0-19.418-8.693-19.418-19.417s8.693-19.417,19.418-19.417c10.723,0,19.416,8.693,19.416,19.417
	S176.07,53.667,165.348,53.667z"/>
                </svg>
    </div>`
}

function c9999(number) {
    const numberStr = number.toString()
    return numberStr.includes('9999')
}

//top helo for 9999
function displayHeadHalo(number) {
    return `<div style="position: absolute; top: 41px; left: 171px;z-index:9999">
            <svg width="272" height="76" viewBox="0 0 272 76" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path fill="#FF9900" d="M49.791,0.05l-2.518,2.425c12.292,0.64,21.393,2.938,21.393,5.676c0,3.233-12.686,5.854-28.333,5.854
		S12,11.384,12,8.151c0-2.803,9.541-5.146,22.279-5.719L31.937,0C13.691,0.798,0,4.144,0,8.151c0,4.603,18.058,8.333,40.333,8.333
		s40.333-3.731,40.333-8.333C80.666,4.222,67.501,0.93,49.791,0.05z"/>
            </svg>
    </div>`
}





function cs5(number) {
    const numberStr = number.toString()
    for (let i = 0; i < numberStr.length - 4; i++) { // Ensure there are at least 3 characters to check
        const substring = numberStr.substring(i, i + 5) // Get the substring of 3 characters
        if (!substring.startsWith('0')) { // Exclude substrings starting with '0'
            const subNum = parseInt(substring, 10)
            const s = Math.sqrt(subNum)
            if (s === Math.floor(s)) { // Check if s is a perfect square
                return "cs5d" // Return a different identifier for 3-digit perfect squares
            }
        }
    }
    return null
}
//used for btcMedal
function displayBtcMedal(number) {
    return `<div style="position: absolute; top: 367px; left: 197px;">
            <svg width="40" height="40" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="19.506" cy="19.507" r="19.506"/>
<path fill="#FF9933" d="M12.392,12.586c-0.979,0.576-2.006,1.056-3.057,1.483c0.122,1.088,0.613,2.124,1.311,2.817
			c0.406,0.405,1.279-0.513,1.982-0.733c0.062-0.019,0.121-0.035,0.182-0.053c0.105-0.064,0.179-0.033,0.22,0.08
			c1.714,3.563,3.434,7.126,5.15,10.689c0.063,0.104,0.038,0.173-0.078,0.21c-0.559,0.281-1.11,0.578-1.68,0.839
			c-0.355,0.164-0.422,0.336-0.273,0.721c0.965,2.519,0.955,2.522,3.367,1.359c0.029-0.013,0.058-0.026,0.085-0.041
			c1.259-0.605,1.312-0.629,1.882,0.694c0.328,0.759,0.686,0.654,1.285,0.382c0.637-0.291,0.836-0.563,0.473-1.254
			c-0.661-1.256-0.618-1.278,0.596-1.861c1.389-0.671,1.384-0.668,2.11,0.718c0.127,0.245,0.135,0.631,0.619,0.54
			c1.105-0.211,1.397-0.717,0.927-1.698c-0.054-0.112-0.098-0.233-0.164-0.337c-0.396-0.623-0.238-0.98,0.501-1.07
			c0.117-0.014,0.274-0.01,0.345-0.08c1.201-1.215,2.265-2.484,2.286-4.344c0.041-3.495-2.269-5.91-5.907-6.118
			c0.435-3.841-2.247-5.705-4.515-5.668c-0.917,0.014-1.302-0.341-1.559-1.084c-0.063-0.176-0.183-0.333-0.239-0.509
			c-0.218-0.703-0.566-0.797-1.244-0.465c-0.677,0.33-0.941,0.631-0.472,1.309c0.158,0.229,0.234,0.514,0.374,0.757
			c0.386,0.667-0.216,0.797-0.567,0.873c-0.69,0.148-1.224,1.258-2.074,0.401c-0.21-0.211-0.318-0.54-0.414-0.838
			c-0.225-0.704-0.58-0.675-1.195-0.405c-0.758,0.329-0.958,0.678-0.477,1.382c0.191,0.278,0.285,0.626,0.423,0.939
			c0.017,0.059,0.034,0.117,0.05,0.176C12.663,12.596,12.531,12.595,12.392,12.586z M25.951,20.78
			c0.629,1.304-0.132,2.572-1.259,3.149c-0.999,0.51-2.026,0.962-3.041,1.439c-0.533-0.165-0.524-0.709-0.729-1.087
			c-0.431-0.797-0.759-1.649-1.204-2.437c-0.325-0.578-0.077-0.775,0.421-0.911c0.838-0.415,1.658-0.872,2.517-1.234
			C24.126,19.079,25.34,19.512,25.951,20.78z M18.4,13.828c0.044-0.046,0.099-0.064,0.161-0.059
			c1.284-0.409,2.272-0.092,2.725,0.877c0.455,0.978,0.078,1.999-1.106,2.635c-0.682,0.366-1.318,0.919-2.174,0.881
			c-0.517-1.138-1.327-2.146-1.481-3.439C17.113,14.348,17.74,14.052,18.4,13.828z"/>
		<path fill="#FF9933" d="M22.945,22.245c-0.063-0.007-0.118,0.013-0.16,0.059l0.088-0.004L22.945,22.245z"/>
</svg>
		
    </div>`
}


// Function to check if a number contains a 4-digit square as a substring
function containsFourDigitSquare(number) {
    const numberStr = number.toString()
    for (let i = 0; i <= numberStr.length - 4; i++) {
        const substring = numberStr.substring(i, i + 4)
        const num = parseInt(substring, 10)
        if (Math.sqrt(num) % 1 === 0) {
            return true
        }
    }
    return false
}

// Determine the range for laser eyes based on the last four digits
function getLaserEyeRange(lastFourDigits) {
    if (lastFourDigits < 4800) {
        return "laser_right"
    } else if (lastFourDigits >= 4800 && lastFourDigits <= 5200) {
        return "laser_crossed"
    } else {
        return "laser_left"
    }
}

// Function to generate SVG for laser eyes based on direction
function displayLaserEyes(eyeDirection) {
    let svgHTML = ""
    switch (eyeDirection) {
        case "laser_left":
            svgHTML = `<div style="position: absolute; top: 230px; left: 0; z-index:999">
                            <svg width="420" height="201" viewBox="0 0 420 201" fill="none" xmlns="http://www.w3.org/2000/svg">
                              <!--  <rect width="284" height="18.5" fill="#FD1935"/>-->
								<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FD1935" points="0,132.964 0,114.611 133.924,0 155,0 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FD1935" points="425.807,132.964 425.807,114.611 291.882,0 270.806,0 "/>
								
                            </svg>
                        </div>`
            break
        case "laser_right":
            svgHTML = `<div style="position: absolute; top: 230px; left: 0;z-index:999">
                        <svg width="420" height="201" viewBox="0 0 420 201" fill="none"  xmlns="http://www.w3.org/2000/svg">
                      <!--  <rect width="262" height="18.5" fill="#FD1935"/>-->
					  <polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FD1935" points="0,132.964 0,114.611 133.924,0 155,0 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FD1935" points="425.807,132.964 425.807,114.611 291.882,0 270.806,0 "/>
                        </svg>
                        </div>`
            break
        case "laser_crossed":
            svgHTML = `<div style="position: absolute; top: 230px; left: 0;z-index:999">
                            <svg width="420" height="201" viewBox="0 0 420 201" fill="none"  xmlns="http://www.w3.org/2000/svg">
							 <polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FD1935" points="0,132.964 0,114.611 133.924,0 155,0 "/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FD1935" points="425.807,132.964 425.807,114.611 291.882,0 270.806,0 "/>
                            </svg>
                    </div>`
            break
    }
    return svgHTML
}


function cs6(number) {
    const numberStr = number.toString()
    for (let i = 0; i < numberStr.length - 5; i++) { // Ensure there are at least 3 characters to check
        const substring = numberStr.substring(i, i + 6) // Get the substring of 3 characters
        if (!substring.startsWith('0')) { // Exclude substrings starting with '0'
            const subNum = parseInt(substring, 10)
            const s = Math.sqrt(subNum)
            if (s === Math.floor(s)) { // Check if s is a perfect square
                return "cs3d" // Return a different identifier for 3-digit perfect squares
            }
        }
    }
    return null
}

function displaydumbbell(number) {
    return `<div style="position: absolute; top: 295px; left: 138px;">
                <svg width="150" height="24" viewBox="0 0 150 24" fill="none" xmlns="http://www.w3.org/2000/svg">
 <rect x="13.153" y="9.062" fill="#660000" width="120.144" height="5.303"/>
<polygon fill="#FF0000" points="-20.39,16.02 -33.278,23.715 -46.167,16.02 -33.278,41.797 "/>
<rect x="6.06" y="2.25" fill="#FF0000" width="7.208" height="19.038"/>
<rect x="2.918" y="5.577" fill="#FF0000" width="3.142" height="12.384"/>
<rect x="1" y="8.164" fill="#FF0000" width="2.033" height="7.486"/>
<rect x="133.343" y="1.879" fill="#FF0000" width="7.208" height="19.038"/>
<rect x="140.552" y="5.207" fill="#FF0000" width="3.142" height="12.384"/>
<rect x="143.579" y="7.794" fill="#FF0000" width="2.033" height="7.486"/>
                </svg>
    </div>`
}


function cf3(number) {
    let numberStr = number.toString()
    let a = 0, b = 1
    let fibNumbers = new Set()

    // Generate 3-digit Fibonacci numbers
    while (b < 1000) { // 1000 is the smallest 4-digit number
        if (b >= 100) { // 100 is the smallest 3-digit number
            fibNumbers.add(b.toString())
        }
        [a, b] = [b, a + b] // Update the Fibonacci sequence
    }

    // Check for 3-digit Fibonacci patterns
    for (let i = 0; i <= numberStr.length - 3; i++) { // Loop through the number string
        let substring = numberStr.substring(i, i + 3)
        if (fibNumbers.has(substring)) {
            return "cf3"
        }
    }

    return null
}

function displayBloodDrips(number) {
    return `<div style="position: absolute; top: 329px; left: 159px;">
                <svg width="36" height="90" viewBox="0 0 36 90" fill="none" xmlns="http://www.w3.org/2000/svg">
                <path fill-rule="evenodd" clip-rule="evenodd" d="M36 18L36 0L18 -7.86805e-07L18 18L18 36L36 36L36 18Z" fill="#FF1E39"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M18 72L18 54L0 54L-7.86805e-07 72L-1.57361e-06 90L18 90L18 72Z" fill="#FF1E39"/>
                </svg>
    </div>`
}


function cf4(number) {
    let numberStr = number.toString()
    let a = 0, b = 1
    let fibNumbers = new Set()

    // Generate 4-digit Fibonacci numbers
    while (b < 10000) { // 10000 is the smallest 5-digit number
        if (b >= 1000) { // 1000 is the smallest 4-digit number
            fibNumbers.add(b.toString())
        }
        [a, b] = [b, a + b] // Update the Fibonacci sequence
    }

    // Check for 4-digit Fibonacci patterns
    for (let i = 0; i <= numberStr.length - 4; i++) { // Loop through the number string
        let substring = numberStr.substring(i, i + 4)
        if (fibNumbers.has(substring)) {
            return "cf4" // Return identifier for 4-digit Fibonacci numbers
        }
    }

    return null
}


function displayHalo(number) {
    return `<div style="position: absolute; top: 41px; left: 146px;">
            <svg width="132" height="18" viewBox="0 0 132 18" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect width="132" height="18" fill="#FFB700"/>
            </svg>
    </div>`
}


function cf5(number) {
    let numberStr = number.toString()
    let a = 0, b = 1
    let fibNumbers = new Set()

    // Generate 5-digit Fibonacci numbers
    while (b < 100000) { // 100000 is the smallest 6-digit number
        if (b >= 10000) { // 10000 is the smallest 5-digit number
            fibNumbers.add(b.toString())
        }
        [a, b] = [b, a + b] // Update the Fibonacci sequence
    }

    // Check for 5-digit Fibonacci patterns
    for (let i = 0; i <= numberStr.length - 5; i++) { // Loop through the number string
        let substring = numberStr.substring(i, i + 5)
        if (fibNumbers.has(substring)) {
            return "cf5"
        }
    }

    return null
}


function displayBrowPiercing(number) {
    return `<div style="position: absolute; top: 174px; left: 320px;">
            <svg width="18" height="18" viewBox="0 0 18 18" fill="none" xmlns="http://www.w3.org/2000/svg">
            <rect x="18" y="18" width="18" height="18" transform="rotate(-180 18 18)" fill="#FFB800"/>
            </svg>

    </div>`
}


function cf6(number) {
    let numberStr = number.toString()
    let a = 0, b = 1
    let fibNumbers = new Set()

    // Generate 6-digit Fibonacci numbers
    while (b < 1000000) { // 1000000 is the smallest 7-digit number
        if (b >= 100000) { // 100000 is the smallest 6-digit number
            fibNumbers.add(b.toString())
        }
        [a, b] = [b, a + b] // Update the Fibonacci sequence
    }

    // Check for 6-digit Fibonacci patterns
    for (let i = 0; i <= numberStr.length - 6; i++) { // Loop through the number string
        let substring = numberStr.substring(i, i + 6)
        if (fibNumbers.has(substring)) {
            return "cf6"
        }
    }

    return null
}


function displayHammer(number) {
    return `<div style="position: absolute; top: 160px; left: 341px;">
                <svg width="84" height="265" viewBox="0 0 84 265" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect x="33" y="265" width="229" height="18" transform="rotate(-90 33 265)" fill="#5A4545"/>
                <path fill-rule="evenodd" clip-rule="evenodd" d="M48 0H0V36H48H84C84 16.1177 67.8822 0 48 0Z" fill="#C6C2D2"/>
                </svg>
    </div>`
}


function cf7(number) {
    let numberStr = number.toString()
    let a = 0, b = 1
    let fibNumbers = new Set()

    // Generate 6-digit Fibonacci numbers
    while (b < 10000000) { // 10000000 is the smallest 8-digit number
        if (b >= 1000000) { // 1000000 is the smallest 7-digit number
            fibNumbers.add(b.toString())
        }
        [a, b] = [b, a + b] // Update the Fibonacci sequence
    }

    // Check for 6-digit Fibonacci patterns
    for (let i = 0; i <= numberStr.length - 7; i++) { // Loop through the number string
        let substring = numberStr.substring(i, i + 7)
        if (fibNumbers.has(substring)) {
            return "cf7"
        }
    }

    return null
}


function displayVial(number) {
    return `<div style="position: absolute; top: 251px; left: 338px;">
                <svg width="36" height="174" viewBox="0 0 36 174" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect x="36" width="174" height="36" transform="rotate(90 36 0)" fill="white" fill-opacity="0.5"/>
                <rect x="36" y="67" width="107" height="36" transform="rotate(90 36 67)" fill="#A2FF00"/>
                </svg>
             </div>`
}

function m12(number) {
    return number % 12 === 0
}
//used for color glass
function displayColorGlass(number) {
    return `<div style="position: absolute; top: 177px; left: 94px;z-index:999;">
                <svg width="238" height="98" viewBox="0 0 238 98" fill="none" xmlns="http://www.w3.org/2000/svg">
               <rect x="149.277" fill-rule="evenodd" clip-rule="evenodd" fill="#FF0000" width="88.057" height="97.442"/>
               <rect fill-rule="evenodd" clip-rule="evenodd" fill="#001EFF" width="87.25" height="97.443"/>
               <rect x="77.639" y="41.284" fill-rule="evenodd" clip-rule="evenodd" fill="#333333" width="81.348" height="13.419"/>
               <rect x="77.639" y="20.572" fill-rule="evenodd" clip-rule="evenodd" fill="#333333" width="81.348" height="13.419"/>
              
                </svg>
    </div>`
}

function m13(number) {
    return number % 13 === 0
}

function displayPearls(number) {
    return `<div style="position: absolute; top: 120px; left: 169px;">
        <svg width="90" height="54" viewBox="0 0 90 54" fill="none" xmlns="http://www.w3.org/2000/svg">
        <rect width="15" height="15" fill="white" fill-opacity="0.3"/>
        <rect x="18" y="18" width="15" height="15" fill="white" fill-opacity="0.3"/>
        <rect x="36" y="36" width="15" height="15" fill="white" fill-opacity="0.3"/>
        <rect x="54" y="18" width="15" height="15" fill="white" fill-opacity="0.3"/>
        <rect x="72" width="15" height="15" fill="white" fill-opacity="0.3"/>
        </svg>
    </div>`
}

function c555(number) {
    const numberStr = number.toString()
    return numberStr.includes('555')
}

//
//function m777(number) {
//    return number % 777 === 0
//}
//used for black glass
function displayThughGlass(number) {
    return `<div style="position: absolute; top: 205px; left: 94px;z-index:99;">
                <svg width="238" height="49" viewBox="0 0 238 49" fill="#000000" xmlns="http://www.w3.org/2000/svg">
              
<polygon fill-rule="evenodd" clip-rule="evenodd" points="0,17.083 7.043,17 7.043,24.906 15.418,24.906 15.418,33 23.256,33 
		23.256,41.125 32.368,41.125 32.368,49.063 80.618,49.063 80.618,41.75 88.243,41.75 88.243,33.375 95.622,33.375 95.622,24.906 
		103.993,24.906 103.993,17 111.993,17 111.993,9.25 119.492,9.25 119.492,0 0,0 	"/>
	<polygon fill-rule="evenodd" clip-rule="evenodd" points="238.985,17.083 231.94,17 231.94,24.906 223.565,24.906 223.565,33 
		215.729,33 215.729,41.125 206.616,41.125 206.616,49.063 158.368,49.063 158.368,41.75 150.743,41.75 150.743,33.375 
		143.362,33.375 143.362,24.906 134.993,24.906 134.993,17 126.993,17 126.993,9.25 119.492,9.25 119.492,0 238.985,0 	"/>
	
		<rect x="15.832" y="16.984" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.698" height="7.698"/>
		<rect x="23.832" y="9.287" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.698" height="7.698"/>
		<rect x="8.134" y="9.287" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.698" height="7.698"/>
		<rect x="23.53" y="25.485" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.698" height="7.698"/>
		<rect x="31.53" y="17.787" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.698" height="7.698"/>
		<rect x="31.53" y="33.183" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.698" height="7.698"/>

		<rect x="145.166" y="17.386" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.697" height="7.698"/>
		<rect x="153.166" y="9.688" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.697" height="7.698"/>
		<rect x="137.468" y="9.688" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.698" height="7.698"/>
		<rect x="152.863" y="25.886" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.697" height="7.698"/>
		<rect x="160.863" y="18.188" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.697" height="7.698"/>
		<rect x="160.863" y="33.584" fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" width="7.697" height="7.698"/>

				
				
                </svg>
    </div>`
}


function m15(number) {
    return number % 15 === 0
}
//Rainbow headband
function displayHeadBand(number) {
    return `<div style="position: absolute; top: 159px; left:95px;">
            <svg width="253" height="22" viewBox="0 0 253 22" fill="none" xmlns="http://www.w3.org/2000/svg">
<rect fill="#0000ff" width="23.671" height="8.219"/>
<rect x="22.833" fill="#A336CC" width="23.671" height="8.219"/>
<rect x="46.504" fill="#ff0000" width="23.671" height="8.219"/>
<rect x="70.175" fill="#ffa500" width="23.671" height="8.219"/>
<rect x="93.846" fill="#ffff00" width="23.671" height="8.219"/>
<rect x="117.517" fill="#008000" width="23.67" height="8.219"/>
<rect x="141.18" fill="#0000ff" width="23.671" height="8.219"/>
<rect x="164.852" fill="#4b0082" width="23.672" height="8.219"/>
<rect x="188.523" fill="#ee82ee" width="23.67" height="8.219"/>
<rect x="212.195" fill="#4b0082" width="23.671" height="8.219"/>

		

            </svg>
    </div>`
}


function m16(number) {
    return number % 16 === 0
}
//this will be used a shoulder medal
function displayShoulderMedal(number) {
    return `<div style="position: absolute; top: 336px; left: 12px;">
                <svg width="406" height="13" viewBox="0 0 406 13" fill="none" xmlns="http://www.w3.org/2000/svg">
              
	<rect x="5.389" fill="#FF0000" width="12.667" height="12.667"/>
	<rect x="18.056" fill="#FFA500" width="12.667" height="12.667"/>
	<rect x="30.723" fill="#FFFF00" width="12.667" height="12.667"/>
	<rect x="43.39" fill="#008000" width="12.667" height="12.667"/>
	<rect x="56.057" fill="#0000FF" width="12.667" height="12.667"/>
	<rect x="68.724" fill="#4B0082" width="12.667" height="12.667"/>
	<rect x="81.391" fill="#EE82EE" width="12.667" height="12.667"/>
	<polygon fill="#666666" points="5.389,6.625 -1.236,6.625 5.389,0 	"/>
	<polygon fill="#666666" points="94.058,6.625 100.683,6.625 94.058,0 	"/>

	<rect x="310.389" fill="#FF0000" width="12.668" height="12.667"/>
	<rect x="323.057" fill="#FFA500" width="12.666" height="12.667"/>
	<rect x="335.723" fill="#FFFF00" width="12.668" height="12.667"/>
	<rect x="348.391" fill="#008000" width="12.666" height="12.667"/>
	<rect x="361.057" fill="#0000FF" width="12.668" height="12.667"/>
	<rect x="373.725" fill="#4B0082" width="12.666" height="12.667"/>
	<rect x="386.391" fill="#EE82EE" width="12.668" height="12.667"/>
	<polygon fill="#666666" points="310.389,6.625 303.764,6.625 310.389,0 	"/>
	<polygon fill="#666666" points="399.059,6.625 405.684,6.625 399.059,0 	"/>

                </svg>
    </div>`
}


function m69(number) {
    return number % 69 === 0
}

//used for earring left
function displayEarRing(number) {
    return `<div style="position: absolute; top: 260px; left: 46px;">
                <svg width="46" height="46" viewBox="0 0 46 46" fill="none" xmlns="http://www.w3.org/2000/svg">
				
		<path fill-rule="evenodd" clip-rule="evenodd" fill="#FF9933" d="M25.275,8.655c1.352-0.818,2.264-2.289,2.264-3.984
	c0-2.58-2.092-4.671-4.671-4.671s-4.671,2.091-4.671,4.671c0,1.904,1.143,3.535,2.776,4.263L0,45.632l23.475-10.053l22.262,10.053
	L25.275,8.655z"/>
				 </svg>
            </div>`
}

function m11(number) {
    return number % 11 === 0
}
//used for snake
function displaySnake(number) {
    return `<div style="position: absolute; top: 280px; left:100px;z-index:9999;">
            <svg width="228" height="58" viewBox="0 0 228 58" fill="none" xmlns="http://www.w3.org/2000/svg">
           <polygon fill="#CC0000" points="18.069,32.976 3,53.218 20.321,34.65 "/>
<polygon fill="#CCCCCC" points="-109.381,194.224 -146.712,156.891 -182.296,192.474 -207.337,166.517 -234.068,198.803 
	-261.315,171.865 -271.69,184.749 -274.67,157.117 -247.039,154.137 -256.222,165.54 -235.337,182.142 -208.337,145.391 
	-182.671,171.057 -146.212,145.391 -109.797,181.807 -66.213,151.057 "/>
	<polygon fill="#CCCCCC" points="40.894,262.667 51.167,262.667 22.5,205 -6.5,262.667 3.893,262.667 22.5,225.666 	"/>
	<polygon fill="#CCCCCC" points="64.56,215.667 74.833,215.667 46.166,273.335 17.166,215.667 27.559,215.667 46.166,252.669 	"/>
	<polygon fill="#CCCCCC" points="112.897,215.668 123.169,215.668 94.502,273.335 65.502,215.668 75.895,215.668 94.502,252.669 "/>
	<polygon fill="#CCCCCC" points="156.729,262.515 167,262.515 138.334,204.847 109.335,262.515 119.728,262.515 138.334,225.513 "/>
<polygon fill="#33CC00" points="189.967,41.999 180.781,29.962 180.714,29.962 160.042,2.562 139.133,29.962 139.202,29.962 
	130.121,41.999 120.935,29.962 120.867,29.962 100.196,2.562 79.286,29.962 79.503,29.962 70.422,41.999 61.236,29.962 
	61.169,29.962 40.498,2.562 25.896,21.696 19.76,15.577 16.854,36.374 37.541,33.306 32.554,28.335 40.498,17.925 49.58,29.962 
	49.512,29.962 70.422,57.362 91.093,29.962 91.01,29.962 100.196,17.925 109.278,29.962 109.21,29.962 130.121,57.362 
	150.792,29.962 150.856,29.962 160.042,17.925 169.126,29.962 169.057,29.962 189.967,57.362 210.639,29.962 226.046,8.34 "/>
</svg>
	
			</svg>
    </div> `
}


function m888(number) {
    return number % 888 === 0
}

function displayTreasureBox(number) {
    return `<div style="position: absolute; top: 302px; left: 150px;">
               <svg width="126" height="79" viewBox="0 0 126 79" fill="none" xmlns="http://www.w3.org/2000/svg">
               <path fill-rule="evenodd" clip-rule="evenodd" fill="#FF9900" d="M0,29.008V12.694C0,5.684,5.683,0,12.694,0h99.966
	c7.011,0,12.694,5.684,12.694,12.694v16.264L0,29.008z"/>
<g>
	<g>
		<rect x="0.093" y="34.159" fill-rule="evenodd" clip-rule="evenodd" fill="#FF9900" width="125.574" height="44.41"/>
	</g>
</g>
<g>
	<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="62.171" cy="55.835" r="16.138"/>
	<g>
		<path fill="#FF9933" d="M56.285,50.109c-0.81,0.476-1.659,0.873-2.529,1.227c0.101,0.9,0.508,1.757,1.085,2.331
			c0.335,0.336,1.058-0.423,1.64-0.606c0.051-0.016,0.099-0.029,0.15-0.043c0.088-0.054,0.148-0.028,0.182,0.066
			c1.418,2.948,2.841,5.896,4.262,8.844c0.052,0.086,0.031,0.143-0.065,0.173c-0.461,0.232-0.918,0.479-1.389,0.694
			c-0.294,0.135-0.349,0.278-0.227,0.596c0.799,2.083,0.791,2.087,2.786,1.125c0.024-0.01,0.048-0.022,0.07-0.034
			c1.042-0.501,1.085-0.521,1.557,0.574c0.271,0.628,0.567,0.542,1.063,0.316c0.527-0.241,0.692-0.466,0.391-1.037
			c-0.547-1.04-0.511-1.058,0.493-1.541c1.148-0.555,1.145-0.553,1.746,0.594c0.105,0.202,0.112,0.521,0.512,0.446
			c0.915-0.175,1.156-0.593,0.767-1.405c-0.044-0.093-0.081-0.193-0.135-0.279c-0.328-0.516-0.197-0.811,0.414-0.885
			c0.097-0.011,0.228-0.008,0.285-0.066c0.994-1.005,1.874-2.056,1.892-3.594c0.034-2.892-1.877-4.889-4.887-5.062
			c0.359-3.177-1.859-4.72-3.735-4.689c-0.758,0.012-1.077-0.282-1.29-0.896c-0.051-0.146-0.151-0.276-0.198-0.421
			c-0.18-0.582-0.468-0.659-1.029-0.384c-0.56,0.272-0.779,0.521-0.391,1.083c0.131,0.19,0.194,0.424,0.31,0.626
			c0.319,0.552-0.179,0.659-0.47,0.722c-0.571,0.123-1.012,1.041-1.716,0.333c-0.173-0.174-0.263-0.447-0.342-0.693
			c-0.186-0.583-0.48-0.559-0.99-0.336c-0.627,0.272-0.792,0.561-0.394,1.144c0.158,0.23,0.235,0.518,0.349,0.777
			c0.014,0.048,0.028,0.097,0.042,0.146C56.511,50.117,56.401,50.116,56.285,50.109z M67.504,56.889
			c0.521,1.079-0.109,2.128-1.041,2.605c-0.827,0.421-1.677,0.795-2.517,1.191c-0.441-0.136-0.434-0.587-0.603-0.9
			c-0.356-0.659-0.628-1.365-0.996-2.015c-0.27-0.479-0.064-0.642,0.348-0.754c0.693-0.343,1.372-0.721,2.082-1.021
			C65.994,55.48,66.998,55.839,67.504,56.889z M61.257,51.137c0.036-0.038,0.082-0.054,0.133-0.049
			c1.062-0.339,1.88-0.076,2.254,0.726c0.376,0.809,0.064,1.654-0.915,2.18c-0.564,0.304-1.091,0.76-1.799,0.729
			c-0.427-0.941-1.098-1.776-1.226-2.846C60.192,51.566,60.711,51.322,61.257,51.137z"/>
		<path fill="#FF9933" d="M65.018,58.101c-0.053-0.006-0.098,0.01-0.133,0.048l0.073-0.003L65.018,58.101z"/>
	</g>
</g>
<g>
	<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="15.501" cy="49.779" r="11.638"/>
	<g>
		<path fill="#FF9933" d="M11.256,45.65c-0.583,0.344-1.196,0.629-1.823,0.885c0.073,0.649,0.366,1.267,0.782,1.681
			c0.242,0.242,0.763-0.305,1.183-0.438c0.037-0.011,0.072-0.021,0.108-0.031c0.063-0.039,0.106-0.02,0.131,0.048
			c1.023,2.126,2.049,4.251,3.073,6.378c0.038,0.062,0.022,0.103-0.047,0.125c-0.333,0.167-0.663,0.345-1.002,0.5
			c-0.212,0.098-0.252,0.2-0.164,0.43c0.576,1.502,0.57,1.505,2.009,0.811c0.018-0.007,0.035-0.016,0.051-0.024
			c0.751-0.361,0.783-0.375,1.123,0.414c0.196,0.453,0.409,0.391,0.767,0.228c0.38-0.173,0.499-0.336,0.282-0.748
			c-0.395-0.75-0.369-0.763,0.356-1.111c0.828-0.4,0.825-0.398,1.259,0.429c0.076,0.146,0.081,0.376,0.369,0.322
			c0.66-0.126,0.834-0.428,0.553-1.014c-0.032-0.067-0.058-0.139-0.098-0.201c-0.236-0.372-0.142-0.585,0.298-0.639
			c0.07-0.008,0.165-0.006,0.206-0.047c0.717-0.725,1.351-1.483,1.364-2.592c0.025-2.085-1.354-3.526-3.524-3.65
			c0.259-2.291-1.341-3.404-2.694-3.381c-0.546,0.008-0.776-0.203-0.93-0.646c-0.037-0.105-0.109-0.199-0.143-0.304
			c-0.129-0.419-0.337-0.475-0.742-0.277c-0.404,0.196-0.562,0.376-0.282,0.781c0.095,0.137,0.14,0.306,0.224,0.451
			c0.23,0.398-0.129,0.476-0.339,0.521c-0.412,0.089-0.73,0.75-1.237,0.24c-0.125-0.126-0.19-0.323-0.247-0.5
			c-0.134-0.42-0.346-0.403-0.714-0.243c-0.452,0.197-0.571,0.405-0.284,0.825c0.114,0.167,0.17,0.374,0.252,0.561
			c0.01,0.035,0.02,0.07,0.03,0.105C11.419,45.656,11.339,45.655,11.256,45.65z M19.347,50.54c0.375,0.778-0.079,1.535-0.751,1.879
			c-0.596,0.304-1.209,0.574-1.815,0.859c-0.318-0.098-0.313-0.423-0.435-0.649c-0.257-0.476-0.453-0.984-0.718-1.454
			c-0.194-0.345-0.046-0.462,0.251-0.544c0.5-0.247,0.989-0.52,1.501-0.736C18.257,49.524,18.982,49.782,19.347,50.54z
			 M14.842,46.391c0.026-0.027,0.059-0.039,0.096-0.035c0.766-0.244,1.355-0.055,1.625,0.523c0.271,0.583,0.046,1.193-0.66,1.572
			c-0.407,0.219-0.787,0.548-1.297,0.526c-0.308-0.679-0.792-1.281-0.884-2.052C14.074,46.701,14.448,46.525,14.842,46.391z"/>
		<path fill="#FF9933" d="M17.554,51.414c-0.038-0.005-0.071,0.007-0.096,0.035l0.053-0.002L17.554,51.414z"/>
	</g>
</g>
<g>
	<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="105.788" cy="50.884" r="11.638"/>
	<g>
		<path fill="#FF9933" d="M101.542,46.755c-0.583,0.344-1.196,0.629-1.823,0.885c0.073,0.649,0.366,1.267,0.782,1.681
			c0.242,0.242,0.763-0.305,1.183-0.438c0.037-0.011,0.072-0.021,0.108-0.031c0.063-0.039,0.106-0.02,0.131,0.048
			c1.023,2.126,2.049,4.251,3.073,6.378c0.038,0.062,0.022,0.103-0.047,0.125c-0.333,0.167-0.663,0.345-1.002,0.5
			c-0.212,0.098-0.252,0.2-0.164,0.43c0.576,1.502,0.57,1.505,2.009,0.811c0.018-0.007,0.035-0.016,0.051-0.024
			c0.751-0.361,0.783-0.375,1.123,0.414c0.196,0.453,0.409,0.391,0.767,0.228c0.38-0.173,0.499-0.336,0.282-0.748
			c-0.395-0.75-0.369-0.763,0.356-1.111c0.828-0.4,0.825-0.398,1.259,0.429c0.076,0.146,0.081,0.376,0.369,0.322
			c0.66-0.126,0.834-0.428,0.553-1.014c-0.032-0.067-0.058-0.139-0.098-0.201c-0.236-0.372-0.142-0.585,0.298-0.639
			c0.07-0.008,0.165-0.006,0.206-0.047c0.717-0.725,1.351-1.483,1.364-2.592c0.025-2.085-1.354-3.526-3.524-3.65
			c0.259-2.291-1.341-3.404-2.694-3.381c-0.546,0.008-0.776-0.203-0.93-0.646c-0.037-0.105-0.109-0.199-0.143-0.304
			c-0.129-0.419-0.337-0.475-0.742-0.277c-0.404,0.196-0.562,0.376-0.282,0.781c0.095,0.137,0.14,0.306,0.224,0.451
			c0.23,0.398-0.129,0.476-0.339,0.521c-0.412,0.089-0.73,0.75-1.237,0.24c-0.125-0.126-0.19-0.323-0.247-0.5
			c-0.134-0.42-0.346-0.403-0.714-0.243c-0.452,0.197-0.571,0.405-0.284,0.825c0.114,0.167,0.17,0.374,0.252,0.561
			c0.01,0.035,0.02,0.07,0.03,0.105C101.706,46.761,101.626,46.76,101.542,46.755z M109.633,51.645
			c0.375,0.778-0.079,1.535-0.751,1.879c-0.596,0.304-1.209,0.574-1.815,0.859c-0.318-0.098-0.313-0.423-0.435-0.649
			c-0.257-0.476-0.453-0.984-0.718-1.454c-0.194-0.345-0.046-0.462,0.251-0.544c0.5-0.247,0.989-0.52,1.501-0.736
			C108.544,50.629,109.269,50.887,109.633,51.645z M105.128,47.496c0.026-0.027,0.059-0.039,0.096-0.035
			c0.766-0.244,1.355-0.055,1.625,0.523c0.271,0.583,0.046,1.193-0.66,1.572c-0.407,0.219-0.787,0.548-1.297,0.526
			c-0.308-0.679-0.792-1.281-0.884-2.052C104.36,47.806,104.734,47.63,105.128,47.496z"/>
		<path fill="#FF9933" d="M107.84,52.519c-0.038-0.005-0.071,0.007-0.096,0.035l0.053-0.002L107.84,52.519z"/>
	</g>
</g>
<g>
	<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="35.788" cy="67.217" r="7.638"/>
	<g>
		<path fill="#FF9933" d="M33.001,64.507c-0.383,0.226-0.785,0.413-1.197,0.581c0.048,0.426,0.241,0.832,0.514,1.103
			c0.159,0.159,0.5-0.2,0.776-0.287c0.024-0.007,0.047-0.013,0.071-0.021c0.042-0.025,0.07-0.013,0.086,0.031
			c0.671,1.396,1.345,2.791,2.017,4.186c0.024,0.041,0.015,0.067-0.031,0.082c-0.219,0.11-0.435,0.226-0.658,0.329
			c-0.139,0.064-0.166,0.131-0.107,0.282c0.378,0.986,0.375,0.988,1.319,0.532c0.011-0.005,0.022-0.01,0.033-0.016
			c0.493-0.237,0.514-0.246,0.737,0.272c0.128,0.297,0.268,0.256,0.503,0.149c0.25-0.114,0.328-0.22,0.185-0.491
			c-0.259-0.492-0.242-0.5,0.233-0.729c0.543-0.263,0.542-0.262,0.826,0.281c0.05,0.096,0.053,0.247,0.242,0.211
			c0.433-0.083,0.547-0.281,0.363-0.666c-0.021-0.044-0.038-0.091-0.064-0.132c-0.155-0.244-0.093-0.384,0.196-0.419
			c0.046-0.005,0.108-0.004,0.135-0.031c0.47-0.476,0.887-0.973,0.895-1.701c0.016-1.369-0.889-2.314-2.313-2.396
			c0.17-1.503-0.88-2.234-1.768-2.219c-0.359,0.006-0.51-0.133-0.61-0.424c-0.024-0.069-0.071-0.131-0.094-0.2
			c-0.085-0.275-0.222-0.312-0.487-0.182c-0.266,0.128-0.369,0.247-0.185,0.512c0.062,0.09,0.092,0.201,0.147,0.296
			c0.151,0.261-0.085,0.313-0.223,0.342c-0.271,0.058-0.479,0.492-0.813,0.157c-0.082-0.083-0.125-0.212-0.162-0.328
			c-0.088-0.276-0.227-0.265-0.468-0.159c-0.297,0.129-0.375,0.266-0.187,0.542c0.075,0.109,0.112,0.245,0.166,0.368
			c0.006,0.023,0.013,0.046,0.02,0.069C33.108,64.511,33.056,64.511,33.001,64.507z M38.312,67.716
			c0.247,0.51-0.052,1.007-0.493,1.233c-0.392,0.199-0.793,0.376-1.191,0.563c-0.208-0.064-0.205-0.278-0.285-0.426
			c-0.169-0.312-0.297-0.646-0.471-0.954c-0.128-0.226-0.031-0.303,0.165-0.357c0.328-0.162,0.649-0.341,0.985-0.483
			C37.597,67.049,38.072,67.219,38.312,67.716z M35.355,64.994c0.017-0.018,0.039-0.025,0.063-0.023
			c0.502-0.16,0.89-0.036,1.067,0.344c0.178,0.383,0.03,0.783-0.433,1.032c-0.267,0.144-0.517,0.36-0.851,0.345
			c-0.202-0.445-0.52-0.84-0.581-1.347C34.851,65.197,35.096,65.081,35.355,64.994z"/>
		<path fill="#FF9933" d="M37.135,68.29c-0.025-0.003-0.046,0.005-0.063,0.023l0.035-0.001L37.135,68.29z"/>
	</g>
</g>
<g>
	<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="38.253" cy="14.527" r="7.638"/>
	<g>
		<path fill="#FF9933" d="M35.467,11.816c-0.383,0.226-0.785,0.413-1.197,0.581c0.048,0.426,0.241,0.832,0.514,1.103
			c0.159,0.159,0.5-0.2,0.776-0.287c0.024-0.007,0.047-0.013,0.071-0.021c0.042-0.025,0.07-0.013,0.086,0.031
			c0.671,1.396,1.345,2.791,2.017,4.186c0.024,0.041,0.015,0.067-0.031,0.082c-0.219,0.11-0.435,0.226-0.658,0.329
			c-0.139,0.064-0.166,0.131-0.107,0.282c0.378,0.986,0.375,0.988,1.319,0.532c0.011-0.005,0.022-0.01,0.033-0.016
			c0.493-0.237,0.514-0.246,0.737,0.272c0.128,0.297,0.268,0.256,0.503,0.149c0.25-0.114,0.328-0.22,0.185-0.491
			c-0.259-0.492-0.242-0.5,0.233-0.729c0.543-0.263,0.542-0.262,0.826,0.281c0.05,0.096,0.053,0.247,0.242,0.211
			c0.433-0.083,0.547-0.281,0.363-0.666c-0.021-0.044-0.038-0.091-0.064-0.132c-0.155-0.244-0.093-0.384,0.196-0.419
			c0.046-0.005,0.108-0.004,0.135-0.031c0.47-0.476,0.887-0.973,0.895-1.701c0.016-1.369-0.889-2.314-2.313-2.396
			c0.17-1.503-0.88-2.234-1.768-2.219c-0.359,0.006-0.51-0.133-0.61-0.424c-0.024-0.069-0.071-0.131-0.094-0.2
			c-0.085-0.275-0.222-0.312-0.487-0.182c-0.266,0.128-0.369,0.247-0.185,0.512c0.062,0.09,0.092,0.201,0.147,0.296
			c0.151,0.261-0.085,0.313-0.223,0.342c-0.271,0.058-0.479,0.492-0.813,0.157c-0.082-0.083-0.125-0.212-0.162-0.328
			c-0.088-0.276-0.227-0.265-0.468-0.159c-0.297,0.129-0.375,0.266-0.187,0.542c0.075,0.109,0.112,0.245,0.166,0.368
			c0.006,0.023,0.013,0.046,0.02,0.069C35.574,11.821,35.522,11.82,35.467,11.816z M40.777,15.026
			c0.247,0.51-0.052,1.007-0.493,1.233c-0.392,0.199-0.793,0.376-1.191,0.563c-0.208-0.064-0.205-0.278-0.285-0.426
			c-0.169-0.312-0.297-0.646-0.471-0.954c-0.128-0.226-0.031-0.303,0.165-0.357c0.328-0.162,0.649-0.341,0.985-0.483
			C40.063,14.359,40.538,14.529,40.777,15.026z M37.821,12.303c0.017-0.018,0.039-0.025,0.063-0.023
			c0.502-0.16,0.89-0.036,1.067,0.344c0.178,0.383,0.03,0.783-0.433,1.032c-0.267,0.144-0.517,0.36-0.851,0.345
			c-0.202-0.445-0.52-0.84-0.581-1.347C37.317,12.506,37.562,12.391,37.821,12.303z"/>
		<path fill="#FF9933" d="M39.601,15.599c-0.025-0.003-0.046,0.005-0.063,0.023l0.035-0.001L39.601,15.599z"/>
	</g>
</g>
<g>
	<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="87.983" cy="15.796" r="7.638"/>
	<g>
		<path fill="#FF9933" d="M85.197,13.085c-0.383,0.226-0.785,0.413-1.197,0.581c0.048,0.426,0.241,0.832,0.514,1.103
			c0.159,0.159,0.5-0.2,0.776-0.287c0.024-0.007,0.047-0.013,0.071-0.021c0.042-0.025,0.07-0.013,0.086,0.031
			c0.671,1.396,1.345,2.791,2.017,4.186c0.024,0.041,0.015,0.067-0.031,0.082c-0.219,0.11-0.435,0.226-0.658,0.329
			c-0.139,0.064-0.166,0.131-0.107,0.282c0.378,0.986,0.375,0.988,1.319,0.532c0.011-0.005,0.022-0.01,0.033-0.016
			c0.493-0.237,0.514-0.246,0.737,0.272c0.128,0.297,0.268,0.256,0.503,0.149c0.25-0.114,0.328-0.22,0.185-0.491
			c-0.259-0.492-0.242-0.5,0.233-0.729c0.543-0.263,0.542-0.262,0.826,0.281c0.05,0.096,0.053,0.247,0.242,0.211
			c0.433-0.083,0.547-0.281,0.363-0.666c-0.021-0.044-0.038-0.091-0.064-0.132c-0.155-0.244-0.093-0.384,0.196-0.419
			c0.046-0.005,0.108-0.004,0.135-0.031c0.47-0.476,0.887-0.973,0.895-1.701c0.016-1.369-0.889-2.314-2.313-2.396
			c0.17-1.503-0.88-2.234-1.768-2.219c-0.359,0.006-0.51-0.133-0.61-0.424c-0.024-0.069-0.071-0.131-0.094-0.2
			c-0.085-0.275-0.222-0.312-0.487-0.182c-0.266,0.128-0.369,0.247-0.185,0.512c0.062,0.09,0.092,0.201,0.147,0.296
			c0.151,0.261-0.085,0.313-0.223,0.342c-0.271,0.058-0.479,0.492-0.813,0.157c-0.082-0.083-0.125-0.212-0.162-0.328
			c-0.088-0.276-0.227-0.265-0.468-0.159c-0.297,0.129-0.375,0.266-0.187,0.542c0.075,0.109,0.112,0.245,0.166,0.368
			c0.006,0.023,0.013,0.046,0.02,0.069C85.304,13.09,85.251,13.089,85.197,13.085z M90.507,16.295
			c0.247,0.51-0.052,1.007-0.493,1.233c-0.392,0.199-0.793,0.376-1.191,0.563c-0.208-0.064-0.205-0.278-0.285-0.426
			c-0.169-0.312-0.297-0.646-0.471-0.954c-0.128-0.226-0.031-0.303,0.165-0.357c0.328-0.162,0.649-0.341,0.985-0.483
			C89.792,15.628,90.268,15.798,90.507,16.295z M87.55,13.572c0.017-0.018,0.039-0.025,0.063-0.023
			c0.502-0.16,0.89-0.036,1.067,0.344c0.178,0.383,0.03,0.783-0.433,1.032c-0.267,0.144-0.517,0.36-0.851,0.345
			c-0.202-0.445-0.52-0.84-0.581-1.347C87.046,13.775,87.292,13.66,87.55,13.572z"/>
		<path fill="#FF9933" d="M89.33,16.868c-0.025-0.003-0.046,0.005-0.063,0.023l0.035-0.001L89.33,16.868z"/>
	</g>
</g>
<g>
	<circle fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" cx="90.454" cy="67.55" r="7.638"/>
	<g>
		<path fill="#FF9933" d="M87.668,64.84c-0.383,0.226-0.785,0.413-1.197,0.581c0.048,0.426,0.241,0.832,0.514,1.103
			c0.159,0.159,0.5-0.2,0.776-0.287c0.024-0.007,0.047-0.013,0.071-0.021c0.042-0.025,0.07-0.013,0.086,0.031
			c0.671,1.396,1.345,2.791,2.017,4.186c0.024,0.041,0.015,0.067-0.031,0.082c-0.219,0.11-0.435,0.226-0.658,0.329
			c-0.139,0.064-0.166,0.131-0.107,0.282c0.378,0.986,0.375,0.988,1.319,0.532c0.011-0.005,0.022-0.01,0.033-0.016
			c0.493-0.237,0.514-0.246,0.737,0.272c0.128,0.297,0.268,0.256,0.503,0.149c0.25-0.114,0.328-0.22,0.185-0.491
			c-0.259-0.492-0.242-0.5,0.233-0.729c0.543-0.263,0.542-0.262,0.826,0.281c0.05,0.096,0.053,0.247,0.242,0.211
			c0.433-0.083,0.547-0.281,0.363-0.666c-0.021-0.044-0.038-0.091-0.064-0.132c-0.155-0.244-0.093-0.384,0.196-0.419
			c0.046-0.005,0.108-0.004,0.135-0.031c0.47-0.476,0.887-0.973,0.895-1.701c0.016-1.369-0.889-2.314-2.313-2.396
			c0.17-1.503-0.88-2.234-1.768-2.219c-0.359,0.006-0.51-0.133-0.61-0.424c-0.024-0.069-0.071-0.131-0.094-0.2
			c-0.085-0.275-0.222-0.312-0.487-0.182c-0.266,0.128-0.369,0.247-0.185,0.512c0.062,0.09,0.092,0.201,0.147,0.296
			c0.151,0.261-0.085,0.313-0.223,0.342c-0.271,0.058-0.479,0.492-0.813,0.157c-0.082-0.083-0.125-0.212-0.162-0.328
			c-0.088-0.276-0.227-0.265-0.468-0.159c-0.297,0.129-0.375,0.266-0.187,0.542c0.075,0.109,0.112,0.245,0.166,0.368
			c0.006,0.023,0.013,0.046,0.02,0.069C87.775,64.844,87.723,64.844,87.668,64.84z M92.978,68.049
			c0.247,0.51-0.052,1.007-0.493,1.233c-0.392,0.199-0.793,0.376-1.191,0.563c-0.208-0.064-0.205-0.278-0.285-0.426
			c-0.169-0.312-0.297-0.646-0.471-0.954c-0.128-0.226-0.031-0.303,0.165-0.357c0.328-0.162,0.649-0.341,0.985-0.483
			C92.263,67.382,92.739,67.552,92.978,68.049z M90.021,65.327c0.017-0.018,0.039-0.025,0.063-0.023
			c0.502-0.16,0.89-0.036,1.067,0.344c0.178,0.383,0.03,0.783-0.433,1.032c-0.267,0.144-0.517,0.36-0.851,0.345
			c-0.202-0.445-0.52-0.84-0.581-1.347C89.518,65.53,89.763,65.414,90.021,65.327z"/>
		<path fill="#FF9933" d="M91.801,68.623c-0.025-0.003-0.046,0.005-0.063,0.023l0.035-0.001L91.801,68.623z"/>
	</g>
</g>
<rect x="51.305" y="3.172" fill-rule="evenodd" clip-rule="evenodd" fill="#FFCC00" width="21.95" height="21.95"/>
<rect x="59.694" y="6.556" fill-rule="evenodd" clip-rule="evenodd" fill="#CC6600" width="5.025" height="13.487"/>
            </svg>
    </div>`
}
function ce7(number) {
    let numberStr = number.toString()
    let exponent = 7
    let power = 1
    while (true) {
        let exponentStr = Math.pow(exponent, power).toString()
        if (numberStr.includes(exponentStr)) {
            return true
        }
        if (Math.pow(exponent, power) > number) break // Stop if the exponent value exceeds the number
        power++
    }
    return false
}

function displayNightVision() {
    return `<div style="position: absolute; top: 182px; left: 104px; z-index:999;">
	<svg width="216" height="92" viewBox="0 0 216 92" fill="none" xmlns="http://www.w3.org/2000/svg">
<path d="M33.133,0C13.608,0,0,18.629,0,18.629v55.294c0,0,10.757,17.857,33.133,17.857
	c22.375,0,33.133-17.857,33.133-17.857V18.629C66.266,18.629,52.659,0,33.133,0z M56.656,68.79c0,0-7.637,12.678-23.523,12.678
	S9.61,68.79,9.61,68.79V23.536c0,0,9.661-13.225,23.523-13.225c13.861,0,23.523,13.225,23.523,13.225V68.79z"/>
<path d="M56.656,68.791c0,0-7.637,12.678-23.523,12.678S9.61,68.791,9.61,68.791V23.537c0,0,9.661-13.225,23.523-13.225
	c13.861,0,23.523,13.225,23.523,13.225V68.791z" fill="#A2FF00" fill-opacity="0.5"/>
<path d="M206.281,68.791c0,0-7.637,12.678-23.523,12.678c-15.885,0-23.523-12.678-23.523-12.678V23.537
	c0,0,9.662-13.225,23.523-13.225s23.523,13.225,23.523,13.225V68.791z" fill="#A2FF00" fill-opacity="0.5"/>
<path d="M182.758,0.031c-19.524,0-33.133,18.629-33.133,18.629v55.294c0,0,10.758,17.857,33.133,17.857
	s33.133-17.857,33.133-17.857V18.66C215.891,18.66,202.283,0.031,182.758,0.031z M206.281,68.821c0,0-7.637,12.678-23.523,12.678
	c-15.885,0-23.523-12.678-23.523-12.678V23.567c0,0,9.662-13.225,23.523-13.225s23.523,13.225,23.523,13.225V68.821z"/>
   
                </svg>
            </div>`
}


//fixed elements
function generateHtmlBasedOnBlockNumber(blockNumber) {
    const lookDir = getLook(blockNumber)
    const lookHtml = generateLookSvg(lookDir)
    let topHelo = c420(blockNumber) ? displayTopHelo() : ""
    let fire = c4a0(blockNumber) ? displayfire() : ""
    let earring = c0(blockNumber) ? displayEarring() : ""
    let earringR = c00(blockNumber) ? displayEarringR() : ""
    let alienEarring = c000(blockNumber) ? displayAlienEarring() : ""
    let alienEarringR = c0000(blockNumber) ? displayAlienEarringR() : ""
    let alienTiara = c00000(blockNumber) ? displayAlienTiara() : ""
    let butterFly = c11(blockNumber) ? displayButterFly() : ""
    let hornRing = c111(blockNumber) ? displayHornRing() : ""
    let flysAlienEarring = c1111(blockNumber) ? displayFlysAlienEarring() : ""
    let flysLaserEyes = c11111(blockNumber) ? displayFlysLaserEyes() : ""
    let bowR = c8a8(blockNumber) ? displayBowR() : ""
    let flowerBadge = c88(blockNumber) ? displayFlowerBadge() : ""
    let headCrown = c888(blockNumber) ? displayHeadCrown() : ""
    let bowChestFlower  = c101(blockNumber) ? displayChestFlower () : ""
    let bowDoubleChestFlower  = c696(blockNumber) ? displayDoubleChestFlower () : ""
    let alienDiamond = cp6(blockNumber) ? displayAlienDiamond() : ""
    let btcMedal = cs5(blockNumber) ? displayBtcMedal() : ""
    let dumbbell = cs6(blockNumber) ? displaydumbbell() : ""
    let fish = c9a9(blockNumber) ? displayFish() : ""
    let cat = c869(blockNumber) ? displayCat() : ""
    let btcKey = c999(blockNumber) ? displayBtcKey() : ""
    let headHalo = c9999(blockNumber) ? displayHeadHalo() : ""
    let bloodDrips = cf3(blockNumber) ? displayBloodDrips() : ""
    let browPiercing = cf4(blockNumber) ? displayBrowPiercing() : ""
    let halo = cf5(blockNumber) ? displayHalo() : ""
    let hammer = cf6(blockNumber) ? displayHammer() : ""
    let vial = cf7(blockNumber) ? displayVial() : ""
    let snake = m11(blockNumber) ? displaySnake() : ""
    let treasureBox = m888(blockNumber) ? displayTreasureBox() : ""
    let nightVision = ce7(blockNumber) ? displayNightVision() : ""
    let colorGlass = m12(blockNumber) ? displayColorGlass() : ""
    let pearls = m13(blockNumber) ? displayPearls() : ""
    let thughGlass = c555(blockNumber) ? displayThughGlass() : ""
    let headBand = m15(blockNumber) ? displayHeadBand() : ""
    let shoulderMedal = m16(blockNumber) ? displayShoulderMedal() : ""
    let earRing = m69(blockNumber) ? displayEarRing() : ""
    const containsSquare = containsFourDigitSquare(blockNumber)
    const eyeDirection = getLaserEyeRange(parseInt(blockNumber.toString().slice(-4)))
    const laserEyes = containsSquare ? displayLaserEyes(eyeDirection) : ""
    const svgsForDigits = generateSvgForDigits(blockNumber)
    const svgsForDigits2 = generateSvgForDigits2(blockNumber)

    //background
    const background = `<div style="position: absolute; top: 0px; left: 0px;">
            <svg width="425" height="425" viewBox="0 0 425 425" fill="none" xmlns="http://www.w3.org/2000/svg">
                <rect width="425" height="425" fill="#201F27"/>
            </svg>
        </div>`;

    //mouth
    const mouth = `<div style="position: absolute; top: 278px; left: 169px; z-index:999;">
            <svg width="97" height="36" viewBox="0 0 97 36" fill="none" xmlns="http://www.w3.org/2000/svg">
              <!--  <path fill-rule="evenodd" clip-rule="evenodd" d="M60 0L90 30V0H113V35H0V0L30 30V0H60Z" fill="#070609"/>-->
			  <polygon fill-rule="evenodd" clip-rule="evenodd" fill="#171717" points="85.5,16.667 74,16.667 66,26.729 18.5,26.729 
	12.833,16.667 0,16.667 0,0 85.5,0 "/>
			 
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" points="2.854,3 12.247,32.562 20.77,3 		"/>
<polygon fill-rule="evenodd" clip-rule="evenodd" fill="#FFFFFF" points="64.664,3 74.057,32.562 82.58,3 		"/>
				
            </svg> 
        </div>`

    //eyes
    const eyes = `
        <div style="position: absolute; top: 190px; left: 23px;z-index:99">
            <svg width="216" height="92" viewBox="0 0 216 92" fill="none" xmlns="http://www.w3.org/2000/svg">
       <rect x="15" y="15" width="5" height="25" transform="rotate(-180 74 38)" fill="white" fill-opacity="0.1"/>
				</svg>
            </div>`

    //teeth//used for wings decoration/
    const teeth = `
        <div style="position: absolute; top: 368px; left:188px; z-index:1;">
                <svg width="50" height="55" viewBox="0 0 50 55" fill="none" xmlns="http://www.w3.org/2000/svg">
               
				
			
                </svg>
            </div>`

//construction
    const htmlContent = background + earring + alienEarring + earringR + alienEarringR + eyes + nightVision + lookHtml + teeth + svgsForDigits2 + mouth + svgsForDigits + fire + topHelo + butterFly + hornRing + flysAlienEarring + headBand + shoulderMedal + flowerBadge + bowR + headCrown + colorGlass + thughGlass + earRing + pearls + bowChestFlower  + bowDoubleChestFlower  + fish + cat + btcKey + headHalo + btcMedal + dumbbell + halo + bloodDrips + hammer + vial + snake + browPiercing + treasureBox + alienDiamond + alienTiara + flysLaserEyes + laserEyes
    document.getElementById('dragonviewer').innerHTML = htmlContent
}
</script>
