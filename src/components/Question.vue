<template>
    <div>
        <div class="question" id="question" @mouseup="getSelectedText">
            <p v-html="savedText"></p>
        </div>
        <button @click="resetSelection">Reset selection</button>
        <button @click="submitSelection">Submit selection</button>
        <h2>Response</h2>
        <div id="response" class="response" v-html="mytext"></div>
        <div v-html="responseText"></div>
    </div>
</template>

<script>
export default {
    name: 'Question',
    props: {},
    data() {
        return {
            // responseText: '',
            savedText:
                'Schaefer’s job, <i>with</i> a Dublin-based solar panel startup, was to write an fe-book <br/> heeeey <br /> hey2',
            startOffset: 0,
            endOffset: 0,
            responses: [
                 { startOffset: 10, endOffset: 20 } ,
                { startOffset: 12, endOffset: 23 } ,
                { startOffset: 2, endOffset: 15 } ,
            ],
        };
    },
    computed: {
        responseText: function() {
            let positionArray = [];
            for(let i=0; i<this.savedText.length; i++) {
                positionArray[i] = 0;
                for(let response of this.responses) {
                    if(response.startOffset <= i && response.endOffset >= i) {
                        positionArray[i]++;
                    }
                }
            }

            const maxCount = Math.max(...positionArray);
            const minCount = Math.min(...positionArray);

            const countRange = maxCount - minCount;

            var outputString = "";
            var insideAnElement = false;
            for (var i = 0; i < this.savedText.length; i++) {
                let currentCharacter = this.savedText[i];
                // we need to not inject new html inside tags, but we need to track our position
                if(currentCharacter == "<") {
                    insideAnElement = true;
                }   
                if(insideAnElement) {
                    outputString = outputString + currentCharacter;
                }
                    
                if(currentCharacter == ">") {
                    insideAnElement = false;
                    continue;
                }

                if(insideAnElement) {
                    continue;
                }

                // super easy way to make a heatmap gradient based on https://stackoverflow.com/questions/12875486/what-is-the-algorithm-to-create-colors-for-a-heatmap
                
                var colorString = null;
                if(positionArray[i] == 0) {
                    colorString = "black";
                }
                else {
                    colorString =
                    "hsl(" + (1.0 - positionArray[i]/countRange) * 240 + ", 100%, 40%)";
                }
                
                outputString =
                    outputString +
                    "<span style='color: " +
                    colorString +
                    "'>" +
                    currentCharacter +
                    '</span>';
            }
            return outputString;


            
        }
    },
    methods: {
        getSelectedText() {
            const mySelection = window.getSelection();
            // console.log(mySelection);

            // if the selection is not empty (aka does not have same start and end point), grab the offsets
            if (!mySelection.isCollapsed) {
                var range = window.getSelection().getRangeAt(0);

                // plain text of selected range (if you want it w/o html)
                var text = window.getSelection();
                console.log(text);
                // document fragment with html for selection
                var fragment = range.cloneContents();
                console.log(fragment);
                // make new element, insert document fragment, then get innerHTML!
                var div = document.createElement('div');
                div.appendChild( fragment.cloneNode(true) );

                // your document fragment to a string (w/ html)! (yay!)
                var html = div.innerHTML;
                var div2 = document.createElement('div');
                div2.innerHTML = this.savedText;
                this.startOffset = div2.innerHTML.indexOf(html);
                this.endOffset = this.startOffset + html.length;

            }
            else {
                //if there isn't a selection, reset what's stored
                this.resetSelection();
            }
        },

        resetSelection() {
            this.startOffset = 0;
            this.endOffset = 0;

            // remove onscreen selection
            window.getSelection().removeAllRanges();
            return;
        },

        submitSelection() {
            let response = {
                    startOffset: this.startOffset,
                    endOffset: this.endOffset,
            };

            this.responses.push(response);
            this.resetSelection();

            return;
        },
    },
};
</script>

<style>
span {
    font-weight: bold;
    font-size: 2em;
    color: black;
}

mark {
    background: black;
}
</style>
