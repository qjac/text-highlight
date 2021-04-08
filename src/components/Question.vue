<template>
    <div>
        <div class="question" id="question" @mouseup="getSelectedText">
            <p v-html="savedText"></p>
        </div>
        <button @click="resetSelection">Reset selection</button>
        <button @click="submitSelection">Submit selection</button>

        <h2>Response</h2>
        <div id="response" class="response" v-html="response"></div>
        <div>this.response = {{ response }}</div>
    </div>
</template>

<script>
export default {
    name: 'Question',
    props: {},
    data() {
        return {
            response: '',
            savedText:
                'Schaefer’s job, with a Dublin-based solar panel startup, was to write an e-book',
            startOffset: 0,
            endOffset: 0,
            responsesArray: {
                student1: { startOffset: 10, endOffset: 20 },
                student2: { startOffset: 12, endOffset: 23 },
                student3: { startOffset: 2, endOffset: 15 },
            },
        };
    },
    computed: {
        mytext: function() {
            var outputString = '';
            // var student1 = {
            //     startOffset: 10,
            //     endOffset: 20,
            // };
            // var student2 = {
            //     startOffset: 12,
            //     endOffset: 23,
            // };
            // var student3 = {
            //     startOffset: 2,
            //     endOffset: 15,
            // };
            // var responses = [student1, student2, student3];
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
            // console.log(histogram);
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
        getSelectedText(e) {
            const mySelection = window.getSelection(e);
            // console.log(mySelection);

            // if the selection is not empty (aka does not have same start and end point), grab the offsets
            if (!mySelection.isCollapsed) {
                const range = mySelection.getRangeAt(0);
                // console.log(range);

                this.startOffset = range.startOffset;
                this.endOffset = range.endOffset;

                console.log(
                    'start offset: ' +
                        this.startOffset +
                        ' | end offset: ' +
                        this.endOffset
                );

                // display selection to selector? If wrapping selection in spans, collect offsets before that happens to keep consistent?

                // this.response = mySelection; // updates this.response value, but doeasn't update view if a new selection is made. Why do they behave differently?
                this.response = range; // updates both this.response value and the view.

                console.log('get: ' + this.response);
                return this.response; // do we need to return? why/why not?
            }
        },

        resetSelection() {
            // button event to clear selection
            console.log('before reset: ' + this.response);
            console.log(window.getSelection());

            // empty stored values
            this.response = '';
            this.startOffset = 0;
            this.endOffset = 0;

            // remove onscreen selection
            window.getSelection().removeAllRanges();

            console.log('reset: ' + this.response);
            console.log(window.getSelection());
        },

        submitSelection() {
            // button event to send response to server

            console.log('before submit: ' + this.response);
            console.log();

            // for now display response on submit
            // push to results array
            // Question? what needs to be in array? Do we need to know who the response is from?
            // console.log(this.mytext);

            // after response sent, resetSelection();
            // console.log('before reset: ' + this.response);
            // console.log(window.getSelection());

            console.log('submit: ' + this.response);
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
