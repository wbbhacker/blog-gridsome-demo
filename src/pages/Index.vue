<template>
  <Layout>
    <!-- Page Header -->
    <header class="masthead" :style="{
      backgroundImage:`url(http://localhost:1337${general.cover.url})`
    }">
      <div class="overlay"></div>
      <div class="container">
        <div class="row">
          <div class="col-lg-8 col-md-10 mx-auto">
            <div class="site-heading">
              <h1>{{ general.title }}</h1>
              <span class="subheading">{{ general.subtitle }}</span>
            </div>
          </div>
        </div>
      </div>
    </header>

    <!-- Main Content -->
    <div class="container">
      <div class="row">
        <div class="col-lg-8 col-md-10 mx-auto">
          <div v-for=" edge in $page.posts.edges " :key="edge.node.id" class="post-preview">
            <g-link :to="'/post/'+edge.node.id">
               <h2 class="post-title">
               {{ edge.node.title }}
              </h2>
            </g-link>
             
            <p class="post-meta">
              Posted by
              <a href="#">Start Bootstrap</a>
             {{ edge.node.created_at }}
            </p>
            <p>
              <span v-for="tag in edge.node.tags" :key="tag.id">
                <g-link :to="'/tag/'+tag.id">{{ tag.title }}</g-link>
                &nbsp;&nbsp;
              </span>
            </p>
            <hr />
          </div>
          <!-- Pager -->
          <!-- <div class="clearfix">
            <a class="btn btn-primary float-right" href="#"
              >Older Posts &rarr;</a
            >
          </div> -->
          <Pager :info="$page.posts.pageInfo"/>
        </div>
      </div>
    </div>

    <hr />
  </Layout>
</template>

<page-query>
query ($page: Int){

  allStrapiGeneral{
    edges{
      node{
          id
          title
          subtitle
          cover{
            id
            url
          }       
      }
    }
  }

  posts:allStrapiPost(perPage: 2, page: $page) @paginate{
     pageInfo {
      totalPages
      currentPage
    }
    edges{
      node{
        id
        title
        content
        created_at
        tags{
          id
          title
        }
      }
    }
  }
}
</page-query>


<script>
import { Pager } from 'gridsome'
export default {
  components: {
    Pager
  },
  name: 'HomePage',
  metaInfo: {
    title: 'Hello, world!'
  },
  computed:{
    general(){
      return this.$page.allStrapiGeneral.edges[0].node
    }
  }
}
</script>

<style>
.home-links a {
  margin-right: 1rem;
}
</style>
