<style>
</style>

<template>
<div>
    <div class="ui container center aligned">
        <div class="ui basic segment huge">
            <h1 class="ui header">
                <i class="large circular asterisk loading icon"></i>
                Messaging Bot Running
            </h1>
        </div>
    </div>
</div>
</template>

<script>
module.exports = {
    data: () => ({ 
        global: shared.state
    }),
    methods: {
    },
    mounted: function() {
        console.log(this.global.onboardStatus, this.$router.path);

        if (this.global.onboardStatus !== 'complete') {
            this.$router.push({ name: 'welcome' });
            return;
        }
        util.fetch.call(this, '/api/onboard/status/v1')
        .then(result => { 
            this.global.onboardStatus = result.theJson.status;
            if (this.global.onboardStatus !== 'complete') {
                this.$router.push({ name: 'welcome' });
            }
        });
        if (!this.global.apiToken) {
            this.$router.push({ name: 'loginTag', query: { forwardTo: this.$router.path }});
            return;
        }
    }
}
</script>