<template>
  <div class="ui grid">
    <div class="six wide column">
    <form class="ui segment large form">
      <div class="ui segment">
        <div class="field">
          <div class="ui right icon input large">
            <input type="text" placeholder="Enter your address"  />
            <i class="dot circle link icon "></i>
          </div>
        </div>
        <div class="field">
          <div class="two fields">
            <div class="field">
              <input type="text" placeholder="Enter Radius Km"  />
            </div>
          </div>
        </div>
        <button class="ui primary button" >Search</button>
      </div>
    </form>
    <div class="ui segment"  style="max-height:500px;overflow:scroll">
   
</div>
</div>
    <div class="ten wide column segment ui"></div>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    
  }
}
</script>
