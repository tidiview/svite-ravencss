<script lang="ts">
  const world = 'world'; // edit world and save to see hmr update
</script>

<style>
/* Check out
https://www.dropbox.com/scl/fi/f6jbc29x52b9z1uhszd91/Meet-the-Raven-one-step-closer-to-container-based-queries.paper
for details */
* {
  box-sizing: border-box;
}

/*
This fork is to demontrate how to avoid problems using the raven and padding. Since padding changes what 100% means between parent and child element, it is sometimes hard to trigger parent and child layout-changes at exactly the same width.
Usinf margins on the children instead of padding on the parent can work around this. But there is some extra locic to ensure margins only on the sides that "face" the parent. But after all, this is just applying the raven more often.
*/

.card {
  --base_size: 100%;
  --breakpoint_wide: 500px;
  --breakpoint_medium: 360px;

  /*  added to select length-valued with the raven (see "step 3" in the article) */
  --very_big_int: 9999;

  /*  Added, since it is used later for --is_medium and --is_small (see "step 1" in the article) */
  --is_wide: calc(clamp(0px, var(--base_size) - var(--breakpoint_wide), 1px));

  --is_medium: calc(
    clamp(0px, var(--base_size) - var(--breakpoint_medium), 1px)
     - var(--is_wide)
  );

  --is_small: calc(1px - (var(--is_medium) + var(--is_wide)));

  /* Setting the padding-values for the breakpoints  */
  --padding_wide: 1rem;
  --padding_medium: 2rem;
  --padding_small: 0.3rem;

  /*  apply "the raven" to calculate the padding values */
  --raven_padding: calc(
    min(var(--is_wide) * var(--very_big_int), var(--padding_wide)) +
      min(var(--is_medium) * var(--very_big_int), var(--padding_medium)) +
      min(var(--is_small) * var(--very_big_int), var(--padding_small))
  );

  /* Setting the image-width for the breakpoints  */
  --img_width_wide: 200px;
  --img_width_medium: 100%;
  --img_width_small: 100%;

  /*  apply "the raven" to calculate the image-width values */
  --raven_img_width: calc(
    min(var(--is_wide) * var(--very_big_int), var(--img_width_wide)) +
      min(var(--is_medium) * var(--very_big_int), var(--img_width_medium)) +
      min(var(--is_small) * var(--very_big_int), var(--img_width_small))
  );

  /* Setting the text width for the breakpoints  */
  --txt_width_wide: calc(
    100% - var(--img_width_wide) - 2 * var(--raven_padding) - 1rem
  ); /* text needs to fit next to image and margin and grid-gap   */

  --txt_width_medium: 100%;
  --txt_width_small: 100%;

  /*  apply "the raven" to calculate the text-width values */
  --raven_txt_width: calc(
    min(var(--is_wide) * var(--very_big_int), var(--txt_width_wide)) +
      min(var(--is_medium) * var(--very_big_int), var(--txt_width_medium)) +
      min(var(--is_small) * var(--very_big_int), var(--txt_width_small))
  );
}

.card {
  /*not using "display:grid;" here 
  use 
  "display:flex;
  flex-wrap:wrap;"
  or use no display at all, but "display:inline-block;" on the child-elements  
  I use a flexbox here, since Chris wanted a gap*/

  display: flex;
  flex-wrap: wrap;
  gap: 1rem;

  width: 100%;
  border: 1px solid #ccc;
  border-radius: 6px;
  /* padding: var(--raven_padding);*/
}

.card-img {
  margin: var(--raven_padding);
  /*  the image only gets a bottom-margin in wide mode */
  margin-bottom: calc(
    min(var(--is_wide) * var(--very_big_int), var(--padding_wide))
  );

  /*  the image only gets a margin-right in small and medium mode */

  margin-right: calc(
    min(var(--is_medium) * var(--very_big_int), var(--padding_medium)) +
      min(var(--is_small) * var(--very_big_int), var(--padding_small))
  );
  /*display: inline-block;*/
  width: var(--raven_img_width);
}
.card-img img {
  border: 1px solid #eee;
  padding: 0.5rem;
  border-radius: 4px;
  border-radius: 2px;
  max-width: 200%;
  width: 100%;
  margin: auto;
  display: block;
}
.card-info {
  margin: var(--raven_padding);
  /*  the text only gets a margin-top in wide mode */
  margin-top: calc(
    min(var(--is_wide) * var(--very_big_int), var(--padding_wide))
  );
  /*  the text only gets a margin-left in small and medium mode */

  margin-left: calc(
    min(var(--is_medium) * var(--very_big_int), var(--padding_medium)) +
      min(var(--is_small) * var(--very_big_int), var(--padding_small))
  );
  /* display: inline-block; */
  width: var(--raven_txt_width);
  vertical-align: top;
}
.card-info h2 {
  margin: 0 0 1rem 0;
}
.card-info p {
  color: #78909c;
}
</style>

<div class="card">
  <div class="card-img">
    <img src='https://images.unsplash.com/photo-1459609336665-6ef3866c8930?ixlib=rb-1.2.1&q=80&fm=jpg&crop=entropy&cs=tinysrgb&w=400&fit=max&ixid=eyJhcHBfaWQiOjE0NTg5fQ' alt='raven' title='raven'></div>
  <div class="card-info">
    <h2>The Raven</h2>
    <p>Lorem, ipsum dolor sit amet consectetur adipisicing elit. Ea, ipsam. Dolores libero cumque alias praesentium eaque sunt. Necessitatibus, asperiores, voluptas id eius similique blanditiis amet molestias veritatis, odit ex non!</p>
  </div>
</div>