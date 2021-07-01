<template>
    <v-app>
       <v-card
        height='100px'
       >
        <v-toolbar app>
         <v-toolbar-title>Шапка сайта</v-toolbar-title>
         <v-spacer></v-spacer>
          Mike
         <v-btn icon href='/'>
            <v-icon>exit_to_app</v-icon>
         </v-btn>
        </v-toolbar>
       </v-card>
        <v-content>
            <v-container>
              <Header :header='header' />
            </v-container>
            <v-container>
              <Table />
            </v-container>
        </v-content>    

        <v-footer app>
            Mike Smirnov Vue Site
        </v-footer>    
    </v-app>
</template>

<script>
import Header from '@/components/Header';
import Table from '@/components/Table';
export default {
  name: 'App',

  components: {
    Header,
    Table
  },

  data: () => ({
      header:'Таблица'
  }),
};
</script>
