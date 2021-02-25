<template
    ><div>
        <div class="question" @mouseup="getSelectedText">
            <h2>When people say, “we have made it through worse before”</h2>
            <h3>— Clint Smith</h3>
            <p>
                all I hear is the wind slapping against the gravestones<br />
                of those who did not make it, those who did not<br />
                survive to see the confetti fall from the sky, those who<br />
                <br />
                did not live to watch the parade roll down the street.<br />
                I have grown accustomed to a lifetime of aphorisms<br />
                meant to assuage my fears, pithy sayings meant to<br />
                <br />
                convey that everything ends up fine in the end. There is no<br />
                solace in rearranging language to make a different word<br />
                tell the same lie. Sometimes the moral arc of the universe<br />
                <br />
                does not bend in a direction that will comfort us.<br />
                Sometimes it bends in ways we don’t expect &amp; there are<br />
                people who fall off in the process. Please, dear reader,<br />
                <br />
                do not say I am hopeless, I believe there is a better future<br />
                to fight for, I simply accept the possibility that I may not<br />
                live to see it. I have grown weary of telling myself lies<br />
                <br />that I might one day begin to believe. We are not all
                left<br />
                standing after the war has ended. Some of us have<br />
                become ghosts by the time the dust has settled.
            </p>

            <hr class="fin" />
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
            this.response = window.getSelection(e).toString();

            // this.response = window.getSelection(e);

            console.log(this.response);
        },
        // handle multiple selects (firefox allows w/ mod key. chrome allows single highlight)
        // unselect text?
    },
};
</script>

<style scoped></style>
