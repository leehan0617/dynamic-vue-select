# dynamic-vue-select
vue-select 컴포넌트에 페이징기능을 추가한 컴포넌트

# Before Use
``` npm install vue-select ```

# Usage

<pre><code>
  <dynamic-vue-select v-model="mc"
                      @search:focus="getListToServer"
                      :options="list"
                      :per-element="5"
                      placeholder="input custom placeholder"
                      label="name">
  </dynamic-vue-select>
</code></pre>
