<template>
    <my-page title="标签" :page="page">
        <div class="article-box">
            <ul class="news-list">
                <li class="item" v-for="tag in tags">
                    <div>
                        {{ tag.name }}
                    </div>
                </li>
            </ul>
        </div>
    </my-page>
</template>

<script>
    /* eslint-disable */
    const moment = window.moment
    import {timeFilter, commonTimeFilter} from '@/util/filter'
    import {cms} from '@/config'

    export default {
        data () {
            return {
                pagination: {
                    page: 1,
                    totalPage: 1
                },
                tags: [],
                page: {
                    menu: [
                        {
                            type: 'icon',
                            icon: 'apps',
                            href: 'https://app.yunser.com?utm_source=tag',
                            target: '_blank',
                            title: '应用'
                        }
                    ]
                }
            }
        },
        mounted() {
            this.hostname = location.hostname
            console.log('cms', cms)
            this.title = document.title = cms.siteName
            this.loadData()

            window.addEventListener('keydown', this.keyDown = e => {
                console.log(e.keyCode)
                if (e.keyCode === 69) {
                    this.$router.push('/admin2')
                }
            })
        },
        destroyed() {
            window.removeEventListener('keydown', this.keyDown)
        },
        methods: {
            loadData() {
                this.$http.get(`/tags?page_size=20&page=${this.pagination.page}`).then(
                    response => {
                        let data = response.data
                        this.tags = data


                        // for (let item of this.article) {
                        //     item.tags = 'ass'
                        // }
                        console.log(response.headers)
                        console.log('X-Total-Page', response.headers['x-total-page'])
                        this.pagination.totalPage = response.headers['x-total-page']
                    },
                    response => {
                        console.log(response)
                    })
            },
            loadMore() {
                this.pagination.page++
                this.loadData()
            },
            commonTimeFilter
        },
        filters: {
            timeFilter,
        }
    }
</script>

<style lang="scss" scoped>
.page-home {
    background-color: #eee;
}
.slogan {
    font-size: 32px;
    margin-bottom: 16px;
}
.article-box {
    background-color: #fff;
}
.news-list {
    .item {
        display: flex;
        align-content: space-between;
        padding: 16px 0;
        // margin-bottom: 16px;
        border-bottom: 1px solid rgba(0, 0, 0, .12);
    }
    .title {
        display: flex;
        align-items: center;
        margin-bottom: 16px;
        font-size: 16px;
        font-weight: bold;
        color: #333;
    }
    .top {
        margin-right: 8px;
    }
    .meta {
        display: flex;
        align-items: center;
        margin-bottom: 8px;
    }
    .avatar {
        display: block;
        width: 16px;
        height: 16px;
        margin-right: 4px;
        border-radius: 50%;
    }
    .user-name {
        color: #666;
        margin-right: 16px;
    }
    .time {
        color: #999;
    }
    .tag {
        margin-right: 8px;
    }
}
.more {
    margin-top: 16px;
}
.icp {
    text-align: center;
    margin-top: 16px;
    margin-bottom: 16px;
}
</style>
