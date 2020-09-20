jQuery(document).ready(function($){

	// temporarily, toggle the main nav buckets
	$('.bucket>a').click(function(e){
		e.preventDefault();
		if( $(this).parent().hasClass('active') ) {
			$('.bucket.active').removeClass('active');

			$('#menu-action-clone').removeClass('active');
		} else {
			$('.bucket.active').removeClass('active');
			$(this).parent().addClass('active');

			$('#menu-action-clone').addClass('active');
		}
	});
	$('body').on('click *', function(e){
		// the click event will bubble up and match every parent selector,
		// so need to check the original event
		var target = $(e.originalEvent.target);
		// Note: this is pretty wildly inefficient and could cause performance issues.
		// The alternative is really, really awful:
		// Make an overlay - visible or otherwise - behind .menu-primary and any click on that closes it.
		if( ! target.closest('.menu-primary').length ) {
			$('.bucket.active').removeClass('active');
		}
	});

	// Clone the action menu (from footer) and append to our main nav
	$('#menu-action')
		.clone()
			.attr('id', '') // we don't want to keep reusing the same unique ID because it won't be unique!
				.appendTo('.bucket .expanded') // add a copy in each expanded region
				.removeClass('menu-horizontal') // change it from menu-horizontal to menu-vertical
				.addClass( 'menu-vertical menu-action' )
				// now clone it into the mobile nav, hidden the rest of the time
				.first()
					.clone()
					.appendTo('#menu-primary')
					.addClass('mobile-action-menu');
	// now clone the non-contact utility menu links into the mobile nav area, except the Contact link, which is displayed up top.
	$('#menu-utility')
		.find('.menu>li>.sub-menu')
			.clone()
				.attr('id', 'mobile-utility-menu')
					.appendTo('#menu-primary')
					.find('.toggle')
						.remove();

	// Mobile navigation
	$('#menu-primary').before('<button class="menu-toggle" data-target="#menu-primary">menu</button>');
	$('.menu-toggle').click(function(event){
		$( $(this).attr('data-target') ).toggleClass('active');
	});

	// toggle the section nav, if we have the requisite reason to navigate to anything.
	if(
		$('#menu-breadcrumb .sub-menu > li')
			.filter('.current-menu-ancestor, .current-menu-item')
			.filter('.menu-item-has-children')
				.length > 0
	) {
		$('#section-nav').before('<button class="toggle-section">In this section</button>');
		$('#menu-breadcrumb').addClass('closed')
		$('.toggle-section').click( function(event){
			$('#menu-breadcrumb').toggleClass('closed');
		});
	}

	// temporarily, toggle the contact region
	$('#menu-utility .toggle > a').click(function(e){
		e.preventDefault();
		$(this).toggleClass('active');
		$('#contact-region').toggleClass('active');
	});

	// find all external links and append _blank to open them in a new window
	$('a').filter(function() {
		return this.hostname && this.hostname !== location.hostname;
	}).attr('target', '_blank');

	// find all red buttons external links and add correct class to them
	$('a.button-red').filter(function() {
		if (this.hostname && this.hostname !== location.hostname) {
			$(this).addClass('button-arrow-external');
		} else {
			$(this).addClass('button-arrow-right');
		}
	});


	// find RSS heading URL and change the link
	$(".widget_rss h2 a[href]").attr('href', 'http://www.northeastern.edu/news/category/science-technology/');
	$('.widget_rss .widget-body').append("<a class='button-yellow button-arrow-right' href='http://www.northeastern.edu/news/'>See all NU News</a>");


	// find all accordion items and set up the toggles
	$('.accordion-item').each( function(index, item){
		// page loads with them open, javascript hides them.
		$(item).addClass('accordion-closed');
		$(item).find('.accordion-label').click( function(event){
			// click it to toggle.
			$(this).parent().toggleClass('accordion-closed');
			$(this).toggleClass('accordion-label-active');
		});
	});

	// add previous/next controls to the carousel
	$('.related-carousel').find('.carousel-item').each(function(index, item){
		// The default is just a link to the big image.
		var imageUrl = $(item).find('> a').attr('href');
		if( index === 0 ) {
			$(item).addClass('first');
		}
		if( index === ( $('.carousel-item').length - 1 ) ) {
			$(item).addClass('last');
		}
		// Once we have the full size image url, load them after the page has rendered. Also, add data-index for prev/next
		$(item)
			.attr('data-index', index)
			.find('.image-wrapper')
				.before('<button class="prev"></button>')
				.after('<button class="next"></button>')
				.prepend('<img src="'+imageUrl+'" alt="" />');
		$(item)
			.on('click', '.close-lightbox, .lightbox', function(event){
				if( this === event.target ) {
					$('.carousel-item').find('.lightbox').addClass('hidden');
					$('html').css('overflow', 'scroll');
				}
			})
			.on('click', '> a', function( event ){
				// Don't link to the full image...
				event.preventDefault();
				// instead, show the lightbox.
				$(item)
					.find('.lightbox')
						.removeClass('hidden');
				$('html').css('overflow', 'hidden');
			})
			.on('click', '.prev, .next', function(event){
				var action = -1;
				if( $(event.target).hasClass('next') ) {
					action = +1;
				}
				console.log( action );
				var current = parseInt( $(event.target).closest('.carousel-item').attr('data-index') );
				console.log( current );
				var newTarget = $('[data-index=' + (current + action) + ']');
				console.log( newTarget );
				if( newTarget.length === 1 ) {
					$('.lightbox').addClass('hidden');
					newTarget.find('.lightbox').removeClass('hidden');
				}
			});
		// close all lightboxes on ESC keypress
		$(document).keyup(function(event) {
			if (event.keyCode == 27) {
				$(item).find('.lightbox').addClass('hidden');
				$('html').css('overflow', 'scroll');
			}
		});
//		$(item)
//			.find('.carousel-items')
//				.after('<div class="carousel-controls"><button class="prev"><span>Previous</span></button><button class="next"><span>Next</span></button></div>');
//		$(item)
//			.attr('data-position', 0)
//				.find( '.prev' )
//					.click(function(e){
//						var pos = parseInt( $(this).closest('.related-carousel').attr('data-position') );
//						if( pos > 0 ) {
//							$(this).closest('.related-carousel').attr('data-position', parseInt(pos) - 1);
//						}
//					})
//			.parent()
//				.find('.next')
//					.click(function(e){
//						var pos = parseInt( $(this).closest('.related-carousel').attr('data-position') );
//						var count = $(this).closest('.related-carousel').find('.carousel-item').length - 1;
//						if( pos < count ) {
//							$(this).closest('.related-carousel').attr('data-position', pos + 1);
//						}
//					})
	});

	// find all publications items and set up the toggles
	$('.publication-content').each( function(index, item){
		// page loads with them open, javascript hides them.
		$(item).addClass('publication-closed').find('.publication-abstract').after('<button class="button-brown button-arrow-down toggle-abstract">Abstract<span></span></button>');
		$(item).find('.toggle-abstract').click( function(event){
			// click it to toggle.
			$(this).closest('.publication-content').toggleClass('publication-closed');
		});
	});

	// find all project items and set up the toggles
	function setProjectToggle() {
		$('.project-content').each( function(index, item){
			// page loads with them open, javascript hides them.
			// check that there's actual content to show
			if ($(item).next('.lightbox').find('.project-abstract').length) {
				$(item).addClass('project-closed').append('<button class="toggle-abstract button-red"></button>');
			}

			$(item).find('.toggle-abstract').click( function(event){
				// click it to toggle.
				$(this).closest('.project-content').
				toggleClass('project-closed')
				.next('.lightbox')
				.toggleClass('hidden');

				$('html').css('overflow', 'hidden');

				// click elsewhere on screen to close lightbox
				$('.lightbox').click(function(event) {
					if (event.target !== this)
						return;
					$('.lightbox').addClass('hidden');
					$('.project-content').addClass('project-closed');
					$('html').css('overflow', 'scroll');
				});

				// click close buttons to close lightbox
				$('.close-lightbox').click( function(event) {
					$('.lightbox').addClass('hidden');
					$('.project-content').addClass('project-closed');
					$('html').css('overflow', 'scroll');
				});

				// check if escape key was pressed to close lightbox
				$(document).keyup(function(event) {
					if (event.keyCode == 27) {
						$('.lightbox').addClass('hidden');
						$('.project-content').addClass('project-closed');
						$('html').css('overflow', 'scroll');
					}
				});
			});
		});
	}
	setProjectToggle();


	// AJAX .load-more for project indices
	$('.related-projects').on('click', '.load-more', function(e){
		e.preventDefault();
		var $el = $(this);
		var href = $el.attr('href') + ' .related-projects .section-inner > *'
		$el.after('<div/>').next('div').load(
			href, // url
			function(){ // callback
				$el.next('div').children().appendTo('.related-projects .section-inner');
				$el.next('div').remove();
				$el.remove();
				setProjectToggle();
			}
		);
	})

	// find all group list containers and set up the toggles
	$('.group-list-container').each( function(index, item){
		$(item).prepend('<button class="toggle-groups button-brown">View Groups</button>');
		$(item).find('.toggle-groups').click( function(event){
			// click it to toggle.
			$(this).toggleClass('active');
			$(this).next('.group-list').toggleClass('active');
		});
	});

	// Toggle .read-more sections opened and closed
	$('.read-more')
		.addClass('read-more-closed')
		.after('<button class="read-more-toggle button-brown">Read <span class="closed">More</span><span class="open">Less</span></button>');
	$('.read-more-toggle').click(function(event){
		$(this)
			.toggleClass('expanded')
			.prev('.read-more')
				.toggleClass('read-more-closed');
	});

	// Toggle the flyout on click - needs refining a bit
	$('#flyout').not('.flyout-disabled').click(function(event){
		$('.active').removeClass('active');
		$(this).toggleClass('flyout-expanded');
	});

	$('.flyout-label').click(function(event){
		$(this).toggleClass('flyout-label-active');
	});

	$('.home-image-toggle').click(function(event){
		$('body').toggleClass('hide-all');
		$('.background-credit').toggleClass('active');
	});


	// Homepage Featured Research toggle
	$('.featured-research .home-square > a').click(function(e){
		e.preventDefault();
		if( $(this).hasClass('show-research') ) {
			$('#research-target').empty();
			$(this).toggleClass('show-research');
		} else {
			$("#research-target").empty().addClass('show-researching');
			$('a.show-research').removeClass('show-research');
			$(this).addClass('show-research')
				.next('div').clone()
				.appendTo("#research-target").toggleClass('toggle-contents');
		}
	});

	// Homepage Featured faculty toggle
	$("a[data-target^='faculty']").click(function(e){
		e.preventDefault();
		if( $(this).hasClass('show-faculty') ) {
			$('#faculty-target').empty();
			$(this).toggleClass('show-faculty');
		} else {
			$("#faculty-target").empty().addClass('show-facultying');
			$('a.show-faculty').removeClass('show-faculty');
			$(this).addClass('show-faculty')
				.next('div').clone()
				.appendTo("#faculty-target").toggleClass('toggle-contents');
		}
	});

	// prevent videos from overlaying the dropdown in IE
	$('iframe').each(function(){
		var url = $(this).attr("src");
		if (url.indexOf("?") > 0) {
			$(this).attr({
			  "src" : url + "&wmode=transparent",
			  "wmode" : "opaque"
			});
		} else {
			$(this).attr({
			  "src" : url + "?wmode=transparent",
			  "wmode" : "opaque"
			});
		}
	});

	/* change catalog button links to use onclick attributes instead of hrefs; PAN's Google Tag Manager tag was adding a string to the URLs which caused the URLs with hashes to break */
	$('a[href*="catalog.northeastern.edu"]').each(function() {
		$(this).removeAttr('target');
  	var href = $(this).attr('href');
  	$(this).attr('onclick', "window.open('" + href + "', '_blank')").removeAttr('href');
	});

// TODO: remove this!
	// for development only, show/hide the <pre> code.
	$('.toggle-pre').click(function(event){
		$(this).nextAll('pre').toggle();
	});

});
