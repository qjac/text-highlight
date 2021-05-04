<template>
    <div>
        <div class="question" id="question" @mouseup="getSelectedText">
            <p v-html="savedText"></p>
        </div>
        <button @click="resetSelection">Reset selection</button>
        <button @click="submitSelection">Submit selection</button>

        <h2>Response</h2>
        <div id="response" class="response" v-html="responseText"></div>
        <!-- <div v-html="responseText"></div> -->
    </div>
</template>

<script>
export default {
    name: 'Question',
    props: {},
    data() {
        return {
            responseText: '',
            savedText:
                'Schaefer’s job, <i>with</i> a Dublin-based solar panel startup, was to write an fe-book <br/> heeeey <br /> hey2',
            startOffset: 0,
            endOffset: 0,
            responses: [
                { student1: { startOffset: 10, endOffset: 20 } },
                { student2: { startOffset: 12, endOffset: 23 } },
                { student3: { startOffset: 2, endOffset: 15 } },
            ],
        };
    },
    computed: {
        // mytext: function() {
            // var outputString = '';

            // console.log('how many responses? ' + this.responses.length);

            // // var responses = this.responses;
            // // console.log(responses);
            // // var histogram = [];
            // // for (var i = 0; i < responses.length; i++) {
            // //     var currentStudent = responses[i];
            // //     for (
            // //         var k = currentStudent.startOffset;
            // //         k < currentStudent.endOffset;
            // //         k++
            // //     ) {
            // //         if (histogram[k]) {
            // //             histogram[k]++;
            // //         } else {
            // //             histogram[k] = 1;
            // //         }
            // //     }
            // // }
            // // console.log('histogram: ' + histogram);
            // // for (i = 0; i < this.savedText.length; i++) {
            // //     if (i >= this.startOffset && i < this.endOffset) {
            // //         var colorString =
            // //             'rgba(100,100,100, ' + (1 * histogram[i]) / 10 + ')';
            // //         outputString =
            // //             outputString +
            // //             "<span style='color: " +
            // //             colorString +
            // //             "'>" +
            // //             this.savedText[i] +
            // //             '</span>';
            // //     } else {
            // //         outputString = outputString + this.savedText[i];
            // //     }
            // // }
            // return outputString;
        // },
    },
    methods: {
        getSelectedText() {
            const mySelection = window.getSelection();

            // if the selection is not empty (aka does not have same start and end point), grab the offsets
            if (!mySelection.isCollapsed) {
                var range = window.getSelection().getRangeAt(0);

                // plain text of selected range (if you want it w/o html)
                // var text = window.getSelection();

                // document fragment with html for selection
                var fragment = range.cloneContents();
                console.log(fragment);

                // make new element, insert document fragment, then get innerHTML!
                var div = document.createElement('div');
                div.appendChild(fragment.cloneNode(true));

                // your document fragment to a string (w/ html)! (yay!)
                var html = div.innerHTML;
                                console.log(html);


                var div2 = document.createElement('div');
                div2.innerHTML = this.savedText;

                this.startOffset = div2.innerHTML.indexOf(html);
                this.endOffset = this.startOffset + html.length;

                console.log(html);

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
                            continue; // what is this keyword?
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
            let key = 'student' + (this.responses.length + 1);

            let response = {
                [key]: {
                    startOffset: this.startOffset,
                    endOffset: this.endOffset,
                },
            };

            this.responses.push(response);
            // after response sent, resetSelection(); or load new view
            this.resetSelection();

            return;
        },

        displayResults() {
            // get full html 
            // step thru character by character
                // pass html tags thru
                // for other characters, ++ for each response that has it in range (data attr? or in css property) and wrap it in a class for styling


            console.table(this.responses);


        }
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
