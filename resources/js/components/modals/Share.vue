<template>
    <modal ref="modal" :title="$t('share.title')">
        <template v-slot:content>
            <p class="mb-2">{{ $t('share.1') }}</p>
            <p class="mb-8">
                <a v-clipboard:copy="url"
                   class="copied link">
                    {{ $t('share.2') }}</a>
                {{ $t('share.3') }}
            </p>

            <share-icons :url="url" :margin-center="true"/>
        </template>
    </modal>
</template>

<script>
    import tippy from 'tippy.js';
    import ShareState from "../../services/ShareState";

    export default {
        data() {
            return {
                url: '',
                shareState: new ShareState,
                copyTippy: null
            }
        },
        mounted() {
            this.$bus.$on('open-share-modal', this.open);
        },
        methods: {
            open() {
                this.url = this.shareState.link();
                this.$refs['modal'].open();
                this.addCopyTippy();
            },
            addCopyTippy() {
                if (this.copyTippy) {
                    return;
                }

                this.copyTippy = tippy('.copied', {
                    trigger: 'click',
                    content: this.$t('Copied'),
                    onShown(tippy) {
                        setTimeout(() => {
                            tippy.hide();
                        }, 1500);
                    }
                });
            }
        }
    }
</script>
