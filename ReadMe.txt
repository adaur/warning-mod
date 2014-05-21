##
##       		   Title:  Warning Mod
##
##   		     Version:  1.0.0
##  	 Works on FluxBB:  1.4.*
##    				Date:  2011-12-19
##                Author: adaur (adaur.underground@gmail.com)
##
##      	 Description: Warning Mod allows you to warn a member before you ban.
##						  After x warnings, the member is banned.
##
## 		  Affected files: viewtopic.php
##						  header.php
##
##			  Affects DB: Yes
##
##            DISCLAIMER: Please note that "mods" are not officially supported by
##                        FluxBB. Installation of this modification is done at your
##                  	  own risk. Backup your forum database and any and all
##                   	  applicable files before proceeding.
##

#
#---------[ 0. UPLOAD ]-------------------------------------------------------------------------------
#

All files from /files folder

#
#---------[ 1. RUN ]-------------------------------------------------------------------------------
#

install_mod.php

#
#---------[ 2. DELETE ]----------------------------------------------------------------------------
#

install_mod.php

#
#---------[ 3. OPEN ]-------------------------------------------------------
#

include/common.php

#
#---------[ 4. FIND ]----------------------------------------------------
#

// Check if we are to display a maintenance message
if ($pun_config['o_maintenance'] && $pun_user['g_id'] > PUN_ADMIN && !defined('PUN_TURN_OFF_MAINT'))
	maintenance_message();

#
#---------[ 5. ADD AFTER ]------------------------------------------------
#

if (file_exists(PUN_ROOT.'lang/'.$pun_user['language'].'/warning_mod.php'))
    require PUN_ROOT.'lang/'.$pun_user['language'].'/warning_mod.php';
else
    require PUN_ROOT.'lang/English/warning_mod.php';

#
#---------[ 6. OPEN ]-------------------------------------------------------
#

header.php

#
#---------[ 7. FIND ]----------------------------------------------------
#

$page_statusinfo[] = '<li><span>'.sprintf($lang_common['Last visit'], format_time($pun_user['last_visit'])).'</span></li>';

#
#---------[ 8. ADD AFTER ]------------------------------------------------
#

if ($pun_user['num_warnings_unread'] > 0)
	$page_statusinfo[] = '<li class="reportlink"><span><strong><a href="warnings.php?uid='.$pun_user['id'].'">'.sprintf($lang_warning['New warning'], $pun_user['num_warnings_unread']).'</a></strong></span></li>';

#
#---------[ 9. OPEN ]-------------------------------------------------------
#

viewtopic.php

#
#---------[ 10. FIND ]----------------------------------------------------
#

u.num_posts, 

#
#---------[ 11. ADD AFTER ]------------------------------------------------
#

u.num_warnings, 

#
#---------[ 12. FIND ]----------------------------------------------------
#

if ($pun_config['o_show_post_count'] == '1' || $pun_user['is_admmod'])
	$user_info[] = '<dd><span>'.$lang_topic['Posts'].' '.forum_number_format($cur_post['num_posts']).'</span></dd>';

#
#---------[ 13. ADD AFTER ]------------------------------------------------
#

if ($cur_post['num_warnings'] > 0)
	$user_info[] = '<dd><span><a href="warnings.php?uid='.$cur_post['poster_id'].'">'.$lang_warning['Warnings'].' '.forum_number_format($cur_post['num_warnings']).'</a></span></dd>';

#
#---------[ 14.FIND ]---------------------------------------------
#

$post_actions[] = '<li class="postreport"><span><a href="misc.php?report='.$cur_post['id'].'">'.$lang_topic['Report'].'</a></span></li>';
$post_actions[] = '<li class="postdelete"><span><a href="delete.php?id='.$cur_post['id'].'">'.$lang_topic['Delete'].'</a></span></li>';
$post_actions[] = '<li class="postedit"><span><a href="edit.php?id='.$cur_post['id'].'">'.$lang_topic['Edit'].'</a></span></li>';
$post_actions[] = '<li class="postquote"><span><a href="post.php?tid='.$id.'&amp;qid='.$cur_post['id'].'">'.$lang_topic['Quote'].'</a></span></li>';

#
#---------[ 15. ADD BEFORE ]-------------------------------------------------
#

if ($cur_post['g_id'] != PUN_ADMIN && $cur_post['g_id'] != PUN_MOD)
	$post_actions[] = '<li class="postreport"><span><a href="admin_loader.php?plugin=AMP_Warning_mod.php&amp;tid='.$id.'&amp;pid='.$cur_post['id'].'&amp;uid='.$cur_post['poster_id'].'">'.$lang_warning['Warn'].'</a></span></li>';
			
#
#---------[ 88. Save your files and upload them; you're done! ]-----------------
#
