# newcomen_functions file - uploaded 17th Aug 2020

<?php

function theme_enqueue_styles() {
    wp_enqueue_style( 'child-style', get_stylesheet_directory_uri() . '/style.css', array( 'avada-stylesheet' ) );
}
add_action( 'wp_enqueue_scripts', 'theme_enqueue_styles' );

function avada_lang_setup() {
	$lang = get_stylesheet_directory() . '/languages';
	load_child_theme_textdomain( 'Avada', $lang );
}
add_action( 'after_setup_theme', 'avada_lang_setup' );

/* Phil's additional coding */

// GOOGLE TAG MANAGER (2nd piece of code) placed after the opening body tag

function my_custom_head_function_for_avada() {
?>

<!-- Google Tag Manager (noscript) -->
<noscript><iframe src="https://www.googletagmanager.com/ns.html?id=GTM-NLJ332P"
height="0" width="0" style="display:none;visibility:hidden"></iframe></noscript>
<!-- End Google Tag Manager (noscript) -->

<?php
}
add_filter( 'avada_before_body_content', 'my_custom_head_function_for_avada' );




/* ACF for Admin Page */

function insert_acf_fields_admin() { 

if (is_page('administration') && !is_paged()) {?>

<!-- Patron Fields -->
<div>
<?php if(get_field('admin_patron')): ?>
	<ul class="adminPatron">
	<?php while(has_sub_field('admin_patron')): ?>
		<li><table><tr><td class="adminPatronTitle"><h2 class="acf-admin"><?php the_sub_field('patron_title'); ?></h2></td><td class="adminPatronName"><a href="<?php the_sub_field('patron_name_link'); ?>" target="_blank"><?php the_sub_field('patron_name'); ?></a></td></tr></table></li>
	<?php endwhile; ?>
	</ul>
</div>

<!-- Council Fields -->
<h2 class="acf-admin">Council</h2>
<div>   
<?php if(get_field('admin_council')): ?>
	<ul class="admin">
	<?php while(has_sub_field('admin_council')): ?>
		<li><div class="adminCouncilTitle"><?php the_sub_field('council_title'); ?></div><div class="adminCouncilName">
<?php if( get_sub_field('council_name_link') ): ?>
	<a href="<?php the_sub_field('council_name_link'); ?>">
<?php endif; ?>
<?php the_sub_field('council_name'); ?>
<?php if( get_sub_field('council_name_link') ): ?></a>
<?php endif; ?>
</div><div class="adminCouncilEmail"><a href="mailto:<?php the_sub_field('council_email'); ?>?subject=Newcomen Website Enquiry"><?php the_sub_field('council_email'); ?></a></div></li>
	<?php endwhile; ?>
	</ul>
<?php endif; ?> 
</div>

<!-- Administration Fields -->
<h2 class="acf-admin">Administration</h2>
<div>
<?php if(get_field('admin_positions')): ?>
	<ul class="admin">
	<?php while(has_sub_field('admin_positions')): ?>
		<li><table><tr><td class="adminPositionTitle"><?php the_sub_field('admin_position_title'); ?></td><td class="adminCouncilName">
<?php if( get_sub_field('admin_position_link') ): ?>
	<a href="<?php the_sub_field('admin_position_link'); ?>">
<?php endif; ?>
		<?php the_sub_field('admin_position_name'); ?>
<?php if( get_sub_field('admin_position_link') ): ?>
	</a>
<?php endif; ?>
</td><td class="adminCouncilEmail"><a href="mailto:<?php the_sub_field('admin_position_email'); ?>?subject=Newcomen Website Enquiry"><?php the_sub_field('admin_position_email'); ?></a></td></tr></table></li>
	<?php endwhile; ?>
	</ul>
<?php endif; ?>
</div>


<?php endif; ?>

<!-- Additonal text below personnel listings -->
<div class="journalText"><?php the_field('additional_text'); ?></div>
<?php }
}

add_action( 'avada_before_additional_page_content', 'insert_acf_fields_admin' );


/* ACF for Contact Page */

function insert_acf_fields_contact() { 

if (is_page('contact') && !is_paged()) {?>

<!-- The Office Administator (from Admin page) -->
<div>
<?php $other_page = 1746;
// check if the repeater field has rows of data
if( have_rows('admin_positions', $other_page) ):
    echo '<ul class="admin">' ;
 	// loop through the rows of data
    while ( have_rows('admin_positions', $other_page) ) : the_row();

        // display a sub field value
        $value = get_sub_field('admin_position_title', $other_page);
        //conditional
        if($value == "The Office Administrator") {?>
             <!-- display the rest of the row fields-->
             <li><div id='contactPositionTitle'><p class="contactPosition">The Administrator</p></div>
             <div class='adminCouncilName'>
				<?php if( get_sub_field('admin_position_link', $other_page) ): ?>
					<a href="<?php the_sub_field('admin_position_link', $other_page); ?>">
				<?php endif; ?>
				<?php the_sub_field('admin_position_name', $other_page); ?>
				<?php if( get_sub_field('admin_position_link', $other_page) ): ?>
					</a>
				<?php endif; ?>
			</div>
    		<div class='adminCouncilEmail'>
    			<a href="mailto:<?php the_sub_field('admin_position_email', $other_page); ?>?subject=Newcomen Website Enquiry">
					<?php the_sub_field('admin_position_email', $other_page); ?></a>
            </div></li>
              <?php }

    endwhile;

else :
    // no rows found
endif; ?>
</ul>
</div>

<!-- Administration Fields (from Admin page) -->
<h2 class="acf-admin">Additional Contacts</h2>
<div>

<?php $other_page = 1746;
// check if the repeater field has rows of data
if( have_rows('admin_positions',$other_page) ): ?>
<?php echo '<ul class="admin">' ;
 	// loop through the rows of data
    while ( have_rows('admin_positions',$other_page) ) : the_row();

        // create conditional to check value of true/false field
        if(get_sub_field('contact_page')){?>
             <!-- display the rest of the row fields-->
             <li><table><tr><td class='adminPositionTitle'><?php the_sub_field('admin_position_title', $other_page); ?></td><td class='adminCouncilName'>
<?php if( get_sub_field('admin_position_link', $other_page) ): ?>
	<a href="<?php the_sub_field('admin_position_link', $other_page); ?>">
<?php endif; ?>
		<?php the_sub_field('admin_position_name', $other_page); ?>
<?php if( get_sub_field('admin_position_link', $other_page) ): ?>
	</a>
<?php endif; ?>
</td><td class='adminCouncilEmail'><a href="mailto:<?php the_sub_field('admin_position_email', $other_page); ?>?subject=Newcomen Website Enquiry"><?php the_sub_field('admin_position_email', $other_page); ?></a></td></tr></table></li>
              <?php }

    endwhile;

else :
    // no rows found
endif; ?>
</ul>
</div>

<!-- Regional Fields (from Newcomen Regional page) -->
<h2 class="acf-admin">Regional Contacts</h2>
<div>

<?php $other_page = 1771;
// check if the repeater field has rows of data
if( have_rows('regional_branches',$other_page) ): ?>
<?php echo '<ul class="admin">' ;
 	// loop through the rows of data
    while ( have_rows('regional_branches',$other_page) ) : the_row();

        // create conditional to check value of true/false field
        if(get_sub_field('branch_email')){?>
             <!-- display the rest of the row fields-->
             <li><div class='adminPositionTitle'>Newcomen <?php the_sub_field('branch_name', $other_page); ?></div>
             <div class='adminCouncilName'><?php if( get_sub_field('branch_city', $other_page) ): ?>(<?php the_sub_field('branch_city', $other_page); ?>)<?php endif; ?></div>
			 <div class='adminCouncilEmail'><a href="mailto:<?php the_sub_field('branch_email', $other_page); ?>?subject=Newcomen Website Enquiry">
			 <?php the_sub_field('branch_email', $other_page); ?></a></div></li>
             <?php }
    endwhile;
else :
    // no rows found
endif; ?>
</ul>
<br/>
<p>Please visit our <a href="/newcomen-regional">Newcomen Regional</a> section to view additional contact details for each region (if available)</p>
</div>
<?php }
}

add_action( 'avada_before_additional_page_content', 'insert_acf_fields_contact' );


/* ACF for The Journal */

function insert_acf_fields_journal() { 

if (is_page('journal-archive') && !is_paged()) {?>

<!--Access to Past Papers Archive -->
<?php if (am4PluginsManager::getAPI()->isLoggedIn() && am4PluginsManager::getAPI()->haveSubscriptions(array(2,8,9,17,19))) {?>
<div>
<p class="archiveAccessTop"><strong><?php $user = am4PluginsManager::getAPI()->getUser();?><?php echo $user["name_f"];?> <?php echo $user["name_l"];?>:</strong> You are currently logged in as a Full Member and have free access to the Journal Archive.
</p>
<p class="archiveAccessButton">
<a href="/wp-content/themes/Avada-Child-Theme/archive/TPS_script_YHET_771757.php" class="archiveButton" target="_blank">
ACCESS TO THE JOURNAL ARCHIVE
</a>
</p>
</div>
<?php } elseif (am4PluginsManager::getAPI()->isLoggedIn() && am4PluginsManager::getAPI()->haveSubscriptions(array(3,4,6,7,18,20))) {?>
<div>

<p class="archiveAccessTop"><strong><?php $user = am4PluginsManager::getAPI()->getUser();?><?php echo $user["name_f"];?> <?php echo $user["name_l"];?>:</strong> You are currently logged in as an Associate Member and therefore do not have privileged access to the Journal Archive.<br></p>
<p>However, you can <a href="https://www.tandfonline.com/loi/yhet20"; target="_blank">click here</a> to view samples of the archive and pay to download items individually. 
</p>
</div>
<?php } else {
	echo '<p class="archiveAccessTop">FULL MEMBERS have free access to the archive when they <a href="/membership/login/?amember_redirect_url='.$_SERVER["REQUEST_URI"].'">login</a>.</p> 
<p>ASSOCIATE and NON MEMBERS can <a href="https://www.tandfonline.com/loi/yhet20" target="_blank">click here</a> to view samples of the archive and pay to download items individually. 
</p>
<p class="archiveAccessButton">
<a href="/membership/signup" class="archiveButton2">
JOIN NOW FOR FULL ARCHIVE ACCESS AND OTHER MEMBER BENEFITS
</a>
</p>';
} ?>

<!-- Call For Papers & Book Review text -->
<div class="journalText"><?php the_field('CallForPapers_text'); ?></div>

<!-- Publishing Team (taken from Administration fields on Admin page) -->
<h2 class="acf-admin">Publishing Team</h2>
<div>
<?php $other_page = 1746;
// check if the repeater field has rows of data
if( have_rows('admin_positions', $other_page) ):
    echo '<ul class="admin">' ;
 	// loop through the rows of data
    while ( have_rows('admin_positions', $other_page) ) : the_row();

        //conditional variables for the word 'journal' in the title field
        $field = get_sub_field('admin_position_title', $other_page);
		$keyword = "Journal";
		$match = strripos ($field, $keyword);
		
        //conditional statement
        if($match !== false) {?>
             <!-- display the rest of the row fields-->
             <li><div class='adminPositionTitle'><?php the_sub_field('admin_position_title', $other_page); ?></div>
             <div class='adminCouncilName'>
				<?php if( get_sub_field('admin_position_link', $other_page) ): ?>
					<a href="<?php the_sub_field('admin_position_link', $other_page); ?>">
				<?php endif; ?>
				<?php the_sub_field('admin_position_name', $other_page); ?>
				<?php if( get_sub_field('admin_position_link', $other_page) ): ?>
					</a>
				<?php endif; ?>
			</div>
    		<div class='adminCouncilEmail'>
    			<a href="mailto:<?php the_sub_field('admin_position_email', $other_page); ?>?subject=Newcomen Website Enquiry">
					<?php the_sub_field('admin_position_email', $other_page); ?></a>
            </div></li>
              <?php }

    endwhile;

else :
    // no rows found
endif; ?>
</ul>
</div>

<!-- Editorial Board -->
<h2 class="acf-admin">Editorial Board</h2>
<div>

<?php
// check if the repeater field has rows of data
if( have_rows('editorial_board') ): ?>
<?php echo '<ul class="admin">' ;
 	// loop through the rows of data
    while ( have_rows('editorial_board') ) : the_row(); {?>
             <!-- display the rest of the row fields-->
             <li><div class='journalEditorialName'>
			 		<?php $link = get_sub_field('editorialboard_link'); if( $link ): ?>
             		<a href="<?php echo $link['url']; ?>" target="<?php echo $link['target']; ?>" title="<?php echo $link['title']; ?>">
             		<?php endif; ?>
			 			<?php the_sub_field('editorialboard_name'); ?>
             		<?php $link = get_sub_field('editorialboard_link'); if( $link ): ?>
             		</a>
             		<?php endif; ?>
             	</div>
                <div class='journalEditorialOrg'>
					<?php the_sub_field('editorialboard_organisation'); ?>
				</div>
                <div class='adminCouncilEmail'>
					<?php the_sub_field('editorialboard_country'); ?>
                </div>
			</li>
              <?php }
    endwhile;
else :
    // no rows found
endif; ?>
</ul>
</div>

<!-- Section about Earlier Volumes of The International Journal -->
<br/>
<div class="journalText"><?php the_field('earlierVolumesText'); ?></div>



<!--<h2 class="acf-admin">Index Files</h2>-->
<div>
	
<?php 
// check if the repeater field has rows of data
if( have_rows('downloads') ): ?>
	
<!-- Index Files of the Journal -->
<?php include('includes/filesize.inc.php') ?>
	
<?php echo '<ul class="journalIndex">' ;
 	// loop through the rows of data
    while ( have_rows('downloads') ) : the_row(); {?>
             <!-- display the rest of the row fields-->
             <li><div class='journalTitle'><?php the_sub_field('volumeTitle'); ?></div>
             <div class='journalPdf'>
				<?php $file = get_sub_field('downloadVolumeLink'); if( $file ): ?>
               			<?php $bytes = $file['filesize'] ?>
						<?php $readableSize = FileSizeConvert($bytes); ?>
             		<a href="<?php echo $file['url']; ?>" target="<?php echo $file['target']; ?>" title="<?php echo $file['title']; ?>">
             	<img src="/wp-content/themes/Avada-Child-Theme/images/pdficon_small.gif"> (<?php echo $readableSize; ?>)</a>
				<?php else: ?>
                N/A
				<?php endif; ?>
             </div>
             <div class='journalBook'>
               <?php if( get_sub_field('paperbackLink') ): ?>
					<a href="<?php the_sub_field('paperbackLink'); ?>">Paperback</a>
                    <?php else: ?>
                N/A
				<?php endif; ?>
             </div>
			 <div class='journalTextDescription'>
             	<?php the_sub_field('volumeDescription'); ?>
             </div>
             </li>
             <?php }
    endwhile;
else :
    // no rows found
endif; ?>
</ul>
</div>
<?php }
}

add_action( 'avada_before_additional_page_content', 'insert_acf_fields_journal' );


/* ACF for The Dickinson Memorial Lectures */

function insert_acf_fields_dickinsonLectures() { 

if (is_page('dickinson-memorial-lecture') && !is_paged()) {?>

<!-- Dickinson Lecture Timeline -->
<h2 class="acf-admin">Past Lectures</h2>
<div>

<?php
// check if the repeater field has rows of data
if( have_rows('lecture_timeline') ): ?>
<?php echo '<ul class="dickinsonLectures">' ;
 	// loop through the rows of data
    while ( have_rows('lecture_timeline') ) : the_row(); {?>
             <li><div class='dickinsonYear'>
					<?php the_sub_field('year'); ?>
                </div>
                <div class="dickinsonSpacer">-</div>
                <div class='dickinsonSpeaker'>
			 		<?php $link = get_sub_field('speaker_link'); if( $link ): ?>
             		<a href="<?php echo $link['url']; ?>" target="<?php echo $link['target']; ?>" title="<?php echo $link['title']; ?>">
             		<?php endif; ?>
			 			<?php the_sub_field('speaker'); ?>
             		<?php $link = get_sub_field('speaker_link'); if( $link ): ?>
             		</a>
             		<?php endif; ?>
             	</div>
                <div class='dickinsonDetail'>
					<?php the_sub_field('lecture_subject'); ?>
				</div>
                
			</li>
              <?php }
    endwhile;
else :
    // no rows found
endif; ?>
</ul>
</div>
<?php }
}

add_action( 'avada_before_additional_page_content', 'insert_acf_fields_dickinsonLectures' );



/* ACF for Newcomen Regional Page */

function insert_acf_fields_regional() {

if (is_page('newcomen-regional') && !is_paged()) {?>

<hr/>
<div class='newcomenRegional'>
<?php if(get_field('regional_branches',$post->ID)): ?>
	<ul class="newcomenRegional">
	<?php while(has_sub_field('regional_branches',$post->ID)): ?>
		<li><div class="regionalBranch"><h2 class="acf-regional">Newcomen <?php the_sub_field('branch_name'); ?></h2><?php if( get_sub_field('branch_city') ): ?><p> (<?php the_sub_field('branch_city'); ?>)</p><?php endif;?><br/><div class="regionDetails"><?php $link = get_sub_field('branch_programme'); if( $link ): ?><a href="<?php echo $link['url']; ?>" target="<?php echo $link['target']; ?>" title="<?php echo $link['title']; ?>"><strong>Programme of Events</strong></a><br/><?php else: echo 'No upcoming events. Please check back later.</br>' ?><?php endif;?><?php if( get_sub_field('branch_email') ): ?><a href="mailto:<?php the_sub_field('branch_email'); ?>?subject=Newcomen Website Enquiry"><?php the_sub_field('branch_email'); ?></a><br/><?php endif;?>
<?php if( have_rows('branch_positions') ): ?>
		<ul class="regionalPositions">
		<?php while( have_rows('branch_positions') ): the_row();?>
			<li><div class="branchPosition"><strong><?php the_sub_field('branch_position'); ?></strong>: <?php if( get_sub_field('position_email') ): ?><a href="mailto:<?php the_sub_field('position_email'); ?>?subject=Newcomen Website Enquiry"><?php endif;?><?php the_sub_field('position_name'); ?><?php if( get_sub_field('position_email') ): ?></a><?php endif;?></div></li>
		<?php endwhile; ?>
		</ul>
<?php endif;?>
        </div></div></li>
<br/>
	<?php endwhile; ?>
	</ul>
</div>
<?php endif; ?>
<?php }
}

add_action( 'avada_before_additional_page_content', 'insert_acf_fields_regional' );


/* ACF for Newcomen Links Page */

function insert_acf_fields_NewcomenLinks() {

if (is_page('newcomen-links') && !is_paged()) {?>

<?php $noInitAjaxForm = true; ?>
<?php if (am4PluginsManager::getAPI()->isLoggedIn() && am4PluginsManager::getAPI()->haveSubscriptions(array(2,3,4,6,7,8,9,17,18,19,20))) {?>
<hr/>
<br/>
<div class='newcomenLinks'>
<?php 
// using normal array
$rows = get_field('front_page_image');
if($rows)
{
	echo '<ul class="newcomenLinks">';
	foreach ($rows as $row)
	{
		echo '<li>' . '<' . 'a href=' . $row['download_url'] .'>' . '<' . 'img src=' . $row['cover_image'] . ' width=\'130\'' . 'height=\'169\'' . 'class=\'shadow\'' . '>' . '</a>'.'<br/>'.'<p class="linksTitle">'. $row['cat_no'] . '</p>' . '</li>';
	}
	echo '</ul>';
}
?>
</div>
<?php } else {
	echo "<hr/><br/><p>Members may " . "<a href='https://www.newcomen.com/membership/login'>login</a>" . " to view and download pdf versions of our quarterly newsletter <strong>Newcomen Links</strong>. Not a member? Why not join by clicking <a href='https://www.newcomen.com/membership/signup'>here?</a></p>";
} ?>

<?php }
}

add_action( 'avada_before_additional_page_content', 'insert_acf_fields_NewcomenLinks' );



/* ACF for Society Business Page */

function insert_acf_fields_SocietyBusiness() {

if (is_page('society-business') && !is_paged()) {?>

<?php include('includes/filesize.inc.php') ?>

<div>
	<?php 
		// check for rows (parent repeater)
		if( have_rows('business_sections') ): ?>
			<?php 
			// loop through rows (parent repeater)
			while( have_rows('business_sections') ): the_row(); ?>
               <div>
                    <h2 class="acf-admin"><?php the_sub_field('section_title'); ?></h2>
					<?php 
					// check for rows (sub repeater)
					if( have_rows('section_details') ): ?>
						<ul class="admin">
						<?php
						// loop through rows (sub repeater)
						while( have_rows('section_details') ): the_row();
							// display each item as a list
							?>
                               <li><div class='businessTitle'>
							   <?php the_sub_field('file_name'); ?><div class="businessSpacer">-</div><?php the_sub_field('date'); ?></div>
                               <div class='journalPdf'>
												<?php $file = get_sub_field('file_download'); if( $file ): ?>
               									<?php $bytes = $file['filesize'] ?>
												<?php $readableSize = FileSizeConvert($bytes) ?>
                                                <a href="<?php echo $file['url']; ?>" target="<?php echo $file['target']; ?>" title="<?php echo $file['title']; ?>">
             									<img src="/wp-content/themes/Avada-Child-Theme/images/pdficon_small.gif"> (<?php echo $readableSize; ?>)
												<?php else: ?>
                								No file to display!
												<?php endif; ?></a>
             								</div>
                               
             					</li>   
						<?php endwhile; ?>
					</ul>
				<?php endif; //if( get_sub_field('section_details') ): ?>
			</div>
		<?php endwhile; // while( has_sub_field('business_sections') ): ?>
	<?php endif; // if( get_field('business_sections') ): ?>	
</div>
<?php }
}

add_action( 'avada_before_additional_page_content', 'insert_acf_fields_SocietyBusiness' );




/* ACF for Membership Pages */

function insert_acf_fields_membershipLists() { ?>

<?php	
/* Check whether we are on this page or a sub page
 *
 * @param int $pid Page ID to check against.
 * @return bool True if we are on this page or a sub page of this page.
 */
function wpdocs_is_tree( $pid ) {      // $pid = The ID of the page we're looking for pages underneath
    global $post;              // load details about this page
 
    $is_tree = false;
    if ( is_page( $pid ) ) {
        $is_tree = true;            // we're at the page or at a sub page
    }
 
    $anc = get_post_ancestors( $post->ID );
    foreach ( $anc as $ancestor ) {
        if ( is_page() && $ancestor == $pid ) {
            $is_tree = true;
        }
    }
    return $is_tree;  // we arn't at the page, and the page is not an ancestor
}
?>

<?php $pid = is_page(array('my-newcomen','the-piston-engine-revolution')) ?>
<?php if (is_page($is_tree) && !is_paged() && !is_page('society-business','journal-archive')) {?> 

<?php include('includes/filesize.inc.php') ?>

<div>
	<?php 
		// ACF for Membership Pages / Membership Lists (Items with optional Links)
		
		// check for rows (parent repeater)
		if( have_rows('membership_lists_links') ): ?>
			<?php 
			// loop through rows (parent repeater)
			while( have_rows('membership_lists_links') ): the_row(); ?>
             <?php $latest = get_sub_field('item_list_new'); ?>
               <div>
                    <h2 class="acf-admin"><?php the_sub_field('item_list_title'); ?></h2>
                    <?php if(get_sub_field('item_list_description')): ?>
                    <div class="itemListDescription"><?php the_sub_field('item_list_description'); ?></div>
                    <?php endif ?>
					<?php 
					// check for rows (sub repeater)
					if( have_rows('item_list_details') ): ?>
						<ul class="admin">
						<?php
						// loop through rows (sub repeater)
						while( have_rows('item_list_details') ): the_row() 
							// display each item as a list
							?>
                              	<?php
    								
									$date = new DateTime(get_sub_field('date'));
    								$now = new DateTime(Date('Y-m-d'));
    								$diff = $now->diff($date);

    								if ($diff->days > $latest):    //Use $diff->days and not $diff->d
								?>
    								<li class='research'
										<?php if( get_sub_field('item_anchor') ): ?>
										id="<?php the_sub_field('item_anchor'); ?>"
										<?php endif;?>
										>
								<?php else: ?>
    								<li class='researchLatest' 
										<?php if( get_sub_field('item_anchor') ): ?>
										id="<?php the_sub_field('item_anchor'); ?>"
										<?php endif;?>
										>
								<?php endif;?>
                               <div class='itemTitle'>
										<?php $link = get_sub_field('link_url'); if( $link ): ?>
										<a href="<?php echo $link['url']; ?>" target="<?php echo $link['target']; ?>" title="<?php echo $link['title']; ?>">
										<?php endif; ?>
										<?php the_sub_field('link_name'); ?>
										<?php $link = get_sub_field('link_url'); if( $link ): ?>
										</a>
										<?php endif; ?><p class='alert'> - <strong>NEW</strong></p>
									</div>
                                    <!--<br/>-->
                                     	<?php if(get_sub_field('link_description')): ?>
                                        <div class="itemDescription">
                                    	<?php echo do_shortcode("[expand title=\"More Info\" swaptitle=\"Close\"]".get_sub_field('link_description')."[/expand]") ?>
                                        </div>
                                       	<?php endif;?>
             					</li>   
						<?php endwhile; ?>
						</ul>
					<?php endif; //if( get_sub_field('item_list_details') ): ?>
				</div>
			<?php endwhile; // while( have_rows('membership_lists_links') ): ?>
		<?php endif; // if( have_rows('membership_lists_links') ): ?>
      
        <?php 
		// ACF for Membership Pages / Membership Lists (Files)
		
		// check for rows (parent repeater)
		if( have_rows('membership_lists_files') ): ?>
			<?php 
			// loop through rows (parent repeater)
			while( have_rows('membership_lists_files') ): the_row(); ?>
               <div>
                    <h2 class="acf-admin"><?php the_sub_field('file_list_title'); ?></h2>
					<?php 
					// check for rows (sub repeater)
					if( have_rows('file_list_details') ): ?>
					<ul class="admin">
						<?php
						// loop through rows (sub repeater)
						while( have_rows('file_list_details') ): the_row();
							// display each item as a list
							?>
                            <li><div class='businessTitle'>
							   <?php the_sub_field('memberlist_file_name'); ?>
                               <?php if( get_field('memberlist_date') ): ?>
                               <div class="businessSpacer">-</div><?php the_sub_field('memberlist_date'); ?><?php endif; ?></div>
                               <div class='journalPdf'>
												<?php $file = get_sub_field('memberlist_file_download'); if( $file ): ?>
               									<?php $bytes = $file['filesize'] ?>
												<?php $readableSize = FileSizeConvert($bytes) ?>
                                                <a href="<?php echo $file['url']; ?>" target="<?php echo $file['target']; ?>" title="<?php echo $file['title']; ?>">
             									<img src="/wp-content/themes/Avada-Child-Theme/images/pdficon_small.gif"> (<?php echo $readableSize; ?>)
												<?php else: ?>
                								No file to display!
												<?php endif; ?></a>
             				   </div>
             				</li>   
						<?php endwhile; ?>
					</ul>
				<?php endif; //if( have_rows('file_list_details') ): ?>
			   </div>
			<?php endwhile; // while( have_rows('membership_lists_files') ): ?>
		<?php endif; // if( have_rows('membership_lists_files') ): ?>	
        	
</div>
<?php }
}


add_action( 'avada_before_additional_page_content', 'insert_acf_fields_membershipLists' );


/* Displaying of City in The Events Calendar Widget */

function insert_tribe_widget_info() {?>
	
	<div class='calenderWidgetCity'>(<?php echo tribe_get_city(); ?>)</div>
    
<?php }

add_action( 'tribe_events_list_widget_after_the_event_title', 'insert_tribe_widget_info' );


/*
 * Add Revision support to WooCommerce Products
 * 
 */

add_filter( 'woocommerce_register_post_type_product', 'cinch_add_revision_support' );

function cinch_add_revision_support( $args ) {
     $args['supports'][] = 'revisions';

     return $args;
}


/**
 * Automatically add IDs to headings such as <h2></h2>
 */
function auto_id_headings( $content ) {

	$content = preg_replace_callback( '/(\<h[1-6](.*?))\>(.*)(<\/h[1-6]>)/i', function( $matches ) {
		if ( ! stripos( $matches[0], 'id=' ) ) :
			$matches[0] = $matches[1] . $matches[2] . ' id="' . sanitize_title( $matches[3] ) . '">' . $matches[3] . $matches[4];
		endif;
		return $matches[0];
	}, $content );

    return $content;

}
add_filter( 'the_content', 'auto_id_headings' );


