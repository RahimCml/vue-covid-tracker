<script>
export default {
  props: ['data']
}
</script>

<template>
    <table class="table">
        <thead>
        <tr>
            <th scope="col">#</th>
            <th scope="col">Ölkə</th>
            <th scope="col">Sağalma</th>
            <th scope="col">Ölüm halları</th>
            <th scope="col">Yoluxma</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="(item, index) in data" :key="index">
            <th scope="row">{{index + 1}}</th>
            <td>{{item.Date | dateFormat}}</td>
            <td>{{item.Confirmed | numberFormat}}</td>
            <td>{{item.Deaths | numberFormat}}</td>
            <td>{{item.Recovered | numberFormat}}</td>
        </tr>
        </tbody>
    </table>
</template>
