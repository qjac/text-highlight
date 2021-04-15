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
        mytext: function() {
            var outputString = '';

            console.log('how many responses? ' + this.responses.length);

            // var responses = this.responses;
            // console.log(responses);
            // var histogram = [];
            // for (var i = 0; i < responses.length; i++) {
            //     var currentStudent = responses[i];
            //     for (
            //         var k = currentStudent.startOffset;
            //         k < currentStudent.endOffset;
            //         k++
            //     ) {
            //         if (histogram[k]) {
            //             histogram[k]++;
            //         } else {
            //             histogram[k] = 1;
            //         }
            //     }
            // }
            // console.log('histogram: ' + histogram);
            // for (i = 0; i < this.savedText.length; i++) {
            //     if (i >= this.startOffset && i < this.endOffset) {
            //         var colorString =
            //             'rgba(100,100,100, ' + (1 * histogram[i]) / 10 + ')';
            //         outputString =
            //             outputString +
            //             "<span style='color: " +
            //             colorString +
            //             "'>" +
            //             this.savedText[i] +
            //             '</span>';
            //     } else {
            //         outputString = outputString + this.savedText[i];
            //     }
            // }
            return outputString;
        },
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
                console.log(html);

                var div2 = document.createElement('div');
                div2.innerHTML = this.savedText;
                console.log(div2.innerHTML);
                console.log(div2.innerHTML.indexOf(html));
                console.log(html.length);
                // ranges.forEach(element => {
                //     console.log(element);
                // });
                this.startOffset = div2.innerHTML.indexOf(html);
                this.endOffset = this.startOffset + html.length;

                // console.log(
                //     'start offset: ' +
                //         this.startOffset +
                //         ' | end offset: ' +
                //         this.endOffset
                // );

                // display selection to selector? If wrapping selection in spans, collect offsets before that happens to keep consistent?
                var outputString = "";
                var insideAnElement = false;
                for (var i = 0; i < div2.innerHTML.length; i++) {
                    if (i >= this.startOffset && i < this.endOffset) {
                    if(div2.innerHTML[i] == "<") {
                        insideAnElement = true;
                    }
                    
                    if(insideAnElement) {
                        outputString = outputString + div2.innerHTML[i];
                    }
                    
                    if(div2.innerHTML[i] == ">") {
                        insideAnElement = false;
                        continue;
                    }

                    if(insideAnElement) {
                        continue;
                    }

                    var colorString =
                        'rgba(100,100,255, ' + (1 * 20) / 10 + ')';
                    outputString =
                        outputString +
                        "<span style='color: " +
                        colorString +
                        "'>" +
                        div2.innerHTML[i] +
                        '</span>';
                } else {
                    outputString = outputString + div2.innerHTML[i];
                }
            }
            this.responseText = outputString;

                // this.responseText = range; // updates both this.response value and the view.

                // console.log('get: ' + this.responseText);
                // return this.responseText; // do we need to return? why/why not?
            } else {
                //if there isn't a selection, reset what's stored
                this.resetSelection();
            }
        },

        resetSelection() {
            // button event to clear selection
            // console.log('before reset: ' + this.responseText);
            // console.log(window.getSelection());

            // empty stored values
            this.responseText = '';
            this.startOffset = 0;
            this.endOffset = 0;

            // remove onscreen selection
            window.getSelection().removeAllRanges();

            // console.log('reset: ' + this.responseText);
            // console.log(window.getSelection());

            return;
        },

        submitSelection() {
            // button event to send response to server

            // console.log('before submit: ' + this.responseText);
            // console.table(this.responses);

            // console.log(
            //     'start offset: ' +
            //         this.startOffset +
            //         ' | end offset: ' +
            //         this.endOffset
            // );

            let key = 'student' + (this.responses.length + 1);
            // console.log(key);
            // array setup: { student1: { startOffset: 10, endOffset: 20 } },
            let response = {
                [key]: {
                    startOffset: this.startOffset,
                    endOffset: this.endOffset,
                },
            };
            // console.log(response);

            this.responses.push(response);

            // console.table(this.responses);

            // after response sent, resetSelection(); or load new view
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
    color: green;
}

mark {
    background: rgba(250, 206, 10, 0.1);
}
</style>
