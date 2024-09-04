<template>
    Title : {{ title }} <br />
    Object : {{ obj }}
    <table>
        <tr>
            <td>
                Test
            </td>
            <td>
                Test2
            </td>
        </tr>
    </table>
    <p>Using text interpolation: {{ rawHtml }}</p>
    <p>Using v-html directive: <span v-html="rawHtml"></span></p>
    <p>Has published books:</p>
    <span>{{ author.books.length > 0 ? 'Yes' : 'No' }}</span>
    <span>{{ publishedBookMessage }}</span>
    <span>{{ publishedBookCount }}</span>
    <span> {{ fuleName }}</span>
    <div :class="{active:isActive}">isActive</div>
    <div class="static" :class="{active:isActive, 'text-danger': hasError}">isActiveTest</div>
</template>
<script>
export default {
    name: "TableList",
    props: {
       title: String, 
       obj: Number,
    },
    data() {
        return {
            rawHtml: "<font color=red>test</font>",
            author: {
                name: 'John Doe',
                books: [
                'Vue 2 - Advanced Guide',
                'Vue 3 - Basic Guide',
                'Vue 4 - The Mystery',
                'Test',
                'Hello',
                ]
            },
            firstName : 'John',
            lastName: 'Doe',
            isActive: true,
            hasError: false,
        }
    },
    computed: {
        publishedBookMessage() {
            return this.author.books.length>0?'Yes':'No'
        },
        publishedBookCount() {
            return this.author.books.length
        },
        fuleName: {
            get() {
                return this.firstName+ " " + this.lastName
            },
            set(newValue) {
                [this.firstName,this.lastName] = newValue.split(' ')
            }
        }
    }
}
</script>