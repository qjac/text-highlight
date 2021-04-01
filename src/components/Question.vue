<template
    ><div>
        <div class="question" id="question" @mouseup="getSelectedText">
            <p>
                Schaefer’s job, with a Dublin-based solar panel startup, was to
                write an e-book
                <em>(“An Irish Homeowner’s Guide to a Zero-Carbon Home”)</em>
                for the Irish public on how solar energy works and other ways to
                make homes more energy efficient.
            </p>
            <p>
                “I thought that was cool to work in marketing, having a break
                from my normal engineering duties and getting to write
                something, because that’s something engineers don’t typically
                get to do,” she says.
            </p>
        </div>
        <h2>Response</h2>
        <p id="response" class="response">
            {{ response }}

            {{ counter }}
        </p>
    </div>
</template>

<script>
export default {
    name: 'Question',
    props: {
        // response: { type: String },
    },
    data() {
        return {
            counter: 0,
            response: '',
        };
    },
    methods: {
        getSelectedText(e) {
            this.counter++;
            const mySelection = window.getSelection(e);

            // console.log(mySelection);

            // console.log('anchor: ' + mySelection.anchorNode); // returns node where selection (mousedown) started
            // console.log('focus: ' + mySelection.focusNode); // returns node where selection (mousedown) ends

            // console.log('isCollapsed: ' + mySelection.isCollapsed); // boolean is the start of the selection in the same place as the end of the selection? is there a selection (true) or just a keypress (false)?

            // console.log('type: ' + mySelection.type);

            // this.response.deleteFromDocument(this.response); // this one's fun

            // surround contents.
            // only works inside of a single node, not when selection spans nodes. throws an error "InvalidStateError: An attempt was made to use an object that is not, or is no longer, usable"
            // it does nest mark tags, but again, slection breaks if second selection is not fully contained
            const range = mySelection.getRangeAt(0);
            // const newParent = document.createElement('mark');
            // const newRange = range.surroundContents(newParent);
            // console.log('new range: ' + range);
            // this.response = newRange;
            // return newRange;

            // similar to surroundContents, but handles crossing nodes and partial nodes better. Still only works as expected within a single node.
            const selectionContents = range.extractContents();
            // console.log(selectionContents);

            // range = document.createRange();
            const newNode = document.createElement('mark');
            newNode.appendChild(selectionContents);
            range.insertNode(newNode);

            // get new question with highlights
            const fullResult = document
                .getElementById('question')
                .cloneNode('deep');
            console.log(fullResult);
            // document.getElementById('response').append(fullResult);

            document
                .getElementById('response')
                .insertAdjacentElement('afterbegin', fullResult);
            // this.response = fullResult;
            // return fullResult;
        },
        // handle multiple selects (firefox allows w/ mod key. chrome allows single highlight). .getRangeAt(0) gets first range in range array
        // unselect text?
    },
};
</script>

<style>
/* scoped styles for span did not appear after making selection. only unscoped worked.*/
span {
    font-weight: bold;
    font-size: 2em;
    color: green;
}

mark {
    background: rgba(250, 206, 10, 0.1);
}
</style>
