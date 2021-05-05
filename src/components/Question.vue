<template>
    <div>
        <div class="question" id="question" @mouseup="getSelectedText">
            <p v-html="savedText"></p>
        </div>
        <button @click="resetSelection">Reset selection</button>
        <button @click="submitSelection">Submit selection</button>

        <h2>Responses</h2>
        <div v-html="combineResponsesHTML"></div>

        <h2>Response</h2>
        <div id="response" class="response" v-html="responseText"></div>
    </div>
</template>

<script>
export default {
    name: 'Question',
    props: {},
    data() {
        return {
            responseText: '',
            // savedText:
            //     'Schaefer’s job, <i>with</i> a Dublin-based solar panel startup, was to write an fe-book <br/> heeeey <br /> hey2',
            savedText: 'Schaefer’s job,',
            responseId: '',
            startOffset: 0,
            endOffset: 0,
            responses: [
                { responseId: 'student1', startOffset: 2, endOffset: 8 },
                { responseId: 'student2', startOffset: 12, endOffset: 23 },
                { responseId: 'student3', startOffset: 2, endOffset: 15 },
            ],
            combineResponsesHTML: '',
        };
    },
    computed: {},
    methods: {
        displayResults() {
            // get full html
            let savedText = this.savedText;
            // console.log(savedText);

            let comboOutputString = '';
            let insideAnElement = false;
            let responsesArray = this.responses;
            // step thru character by character
            for (var i = 0; i < savedText.length; i++) {
                // console.log(i + ': ' + savedText[i]);

                // pass html tags thru
                if (savedText[i] == '<') {
                    insideAnElement = true;
                }

                if (insideAnElement) {
                    comboOutputString = comboOutputString + savedText[i];
                }

                if (savedText[i] == '>') {
                    insideAnElement = false;
                    continue;
                }

                if (insideAnElement) {
                    continue;
                }

                let counter = 0;

                // loop thru each response and grab offsets
                responsesArray.forEach(function(response) {
                    if (i >= response.startOffset && i < response.endOffset) {
                        // the `i` refers to index of character array

                        // how many responses highlighted this character?
                        counter++;
                    }

                    // console.log('counter: ' + counter);
                });

                if (counter === 0) {
                    comboOutputString = comboOutputString + savedText[i];
                } else {
                    comboOutputString =
                        comboOutputString +
                        "<span class='highlight'>" +
                        savedText[i] +
                        '</span>';
                }

                // wrap in span if counter === 1?? or wrap all chars in span
                // if counter > 1, it's already wrapped so no need to wrap it.
                // counter ++ a style tag or variables in vue css????
                // for other characters, ++ for each response that has it in range (data attr? or in css property) and wrap it in a class for styling
            }
            // console.log(comboOutputString);
            this.combineResponsesHTML = comboOutputString;
        },
        getSelectedText() {
            const mySelection = window.getSelection();

            // if the selection is not empty (aka does not have same start and end point), grab the offsets
            if (!mySelection.isCollapsed) {
                var range = window.getSelection().getRangeAt(0);

                // plain text of selected range (if you want it w/o html)
                // var text = window.getSelection();

                // document fragment with html for selection
                var fragment = range.cloneContents();

                // make new element, insert document fragment, then get innerHTML!
                var div = document.createElement('div');
                div.appendChild(fragment.cloneNode(true));

                // your document fragment to a string (w/ html)! (yay!)
                var html = div.innerHTML;

                var div2 = document.createElement('div');
                div2.innerHTML = this.savedText;

                this.startOffset = div2.innerHTML.indexOf(html);
                this.endOffset = this.startOffset + html.length;

                var outputString = '';
                var insideAnElement = false;

                for (var i = 0; i < div2.innerHTML.length; i++) {
                    if (i >= this.startOffset && i < this.endOffset) {
                        if (div2.innerHTML[i] == '<') {
                            insideAnElement = true;
                        }

                        if (insideAnElement) {
                            outputString = outputString + div2.innerHTML[i];
                        }

                        if (div2.innerHTML[i] == '>') {
                            insideAnElement = false;
                            continue;
                        }

                        if (insideAnElement) {
                            continue;
                        }
                        outputString =
                            outputString +
                            "<span class='highlight-text'>" +
                            div2.innerHTML[i] +
                            '</span>';
                    } else {
                        outputString = outputString + div2.innerHTML[i];
                    }
                }
                this.responseText = outputString;
            } else {
                //if there isn't a selection, reset what's stored
                this.resetSelection();
            }
        },

        resetSelection() {
            // empty stored values
            this.responseText = '';
            this.startOffset = 0;
            this.endOffset = 0;

            // remove onscreen selection
            window.getSelection().removeAllRanges();

            return;
        },

        submitSelection() {
            this.responseId = 'student' + (this.responses.length + 1);

            let response = {
                responseId: this.responseId,
                startOffset: this.startOffset,
                endOffset: this.endOffset,
            };

            this.responses.push(response);

            // after response sent, resetSelection(); or load new view (maybe a 'your response is submitted message)
            this.resetSelection();

            this.displayResults();

            return;
        },
    },
};
</script>

<style>
span {
    /* font-weight: bold; */
    font-size: 2em;
    /* color: green; */
}

mark {
    background: rgba(250, 206, 10, 0.1);
}

.highlight-text {
    background: rgba(250, 10, 190, 0.3);
}
</style>
